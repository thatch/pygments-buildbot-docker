from base/archlinux:latest

run mkdir /tmp/pkg
# TODO I should make an arch-devel images that's capable of building the
# packages without an arch host.  To get there:
## all in group is <enter>, then yes to install.
#run echo -e "\ny\n" | pacman -S base-devel
#run yes | pacman -S --asdeps sqlite3 tk

# This next line pulls in the rolling releases; they don't update the
# base/archlinux image often enough, and mirrors are really fast to delete old
# versions of packages.  So we fetch here, but this requires manual cache
# invalidation on the docker side.
run pacman -Sy
run yes | pacman -S jdk8-openjdk
run yes | pacman -Su
run yes | pacman -S python mercurial python2-pip python2-nose python-nose

# Install our special (older and/or ucs2) versions.
copy ?ython*.pkg.tar.xz /tmp/pkg/
run yes | pacman -U /tmp/pkg/*
run rm -rf /tmp/pkg
# TODO fix in package
run ln -s /usr/bin/jython27 /usr/bin/jython2.7

# TODO pypy

# TODO get nose extracted someplace and just use that. For now we symlink.
run s=/usr/lib/python2.7/site-packages; \
    for t in /usr/lib/python2.6/site-packages \
    /opt/jython27/Lib/site-packages; do \
        ln -s $s/nose $t/nose; \
    done; \
    s=/usr/lib/python3.4/site-packages; \
    for t in /usr/lib/python3.3/site-packages; do \
        ln -s $s/nose $t/nose; \
    done

run find /var/cache/pacman/pkg/ -type f -delete

# not installed by pip, needs gcc for raiser.o
run yes | pacman -S buildbot-slave make
run useradd -m -G users buildbot -s /bin/bash
user buildbot
# $HOME is still / even after ther 'user' command :(
workdir /home/buildbot
run echo 'python.cachedir=/home/buildbot/.jythoncachedir' > .jython
# prepopulate sys-package-mgr cache.
run jython -c 'import sys'

