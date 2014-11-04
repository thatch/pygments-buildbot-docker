all: fetch build copy
.PHONY: all

fetch:
	for i in 26 33; do \
		curl https://aur.archlinux.org/packages/py/python$$i/python$$i.tar.gz | \
		tar -xz; \
	done
	curl https://aur.archlinux.org/packages/jy/jython27/jython27.tar.gz | tar -xz

.PHONY: fetch

ALLPKGS=$(shell for f in $(ls -d */); do (. $$f/PKGBUILD; echo $$pkgname/$$pkgname-$$pkgver-$$pkgrel.pkg.tar.xz); done)

packages: $(ALLPKGS) 
.PHONY: packages

build: python26 python33 jython27

# No python27-ucs2 because it conflicts with the builtin python2.7; we get this
# coverage on ubuntu right now.  Easy to install to a different prefix though.
# But needs new fetch/patch step.

python26:
	cd python26 && makepkg
#python27-ucs2:
#	cd python27-ucs2 && makepkg
python33:
	cd python33 && makepkg
jython27:
	cd jython27 && makepkg

.PHONY: python26 python33 jython27
#python27-ucs2

copy:
	cp */*.pkg.tar.xz ../bbtest-arch/
.PHONY: copy
