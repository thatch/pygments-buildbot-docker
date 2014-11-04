Docker build information for Pygments buildbots
===============================================

These are the Dockerfiles that will create builders used by Pygments.  The
primary goal of this over the drone.io builds is that we can have a persistent
image with multiple versions of Python preinstalled, also multiple distros with
their available versions, bugs and all.

* Arch
  * 2.6 from AUR
  * 2.7
  * Jython 2.7b3 on Java 8
  * 3.3 from AUR
  * 3.4
* Centos
  * 2.7
  * 3.3 from SCL
* Ubuntu
  * 2.7
  * 2.7 from source with --enable-unicode=ucs2
  * Jython 2.7b3 on Java 7
  * 3.4

(Also on the Buildbot, but not built from these)

* WinXP
  * 2.7 (default, narrow)
  * 3.4 (default, narrow)
* ARM Ubuntu
  * 2.7
* PPC Debian
  * ???

Building
--------

First, build $image-base, e.g.::

    cd bbtest-arch && make bbtest-arch-base

Then, if you want this to report to a buildmaster automatically, ensure that the
`pygments_buildbot` package is on your `$PYTHONPATH`.  Then::

    cd bbtest-arch && make bbtest-arch-auto
    # or
    cd bbest-arch && make bbtest-arch-manual NAME=foo PW=foo


Rebuilding Arch Packages
------------------------

Right now you need an Arch host.  This will build them and copy to the build
directory for bbtest-arch.

::

    cd src && make build copy
