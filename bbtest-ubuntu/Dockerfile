# trusty
from ubuntu:latest

run apt-get update
# We --no-install-recommends to try to avoid mercurial->wish->x11, but
# openjdk-7-jre-headless still pulls in x11-common.
run DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
    python-pip wget python-dev zlib1g-dev libssl-dev \
    mercurial buildbot-slave \
    openjdk-7-jre-headless \
    python-nose python3-nose
# pulls in gcc and make
run DEBIAN_FRONTEND=noninteractive apt-get build-dep -y python

add Python-2.7.8.tar.xz /tmp
workdir /tmp/Python-2.7.8

run ./configure --enable-unicode=ucs2 --prefix=/opt/python27-ucs2 && \
    make -j6 && \
    make altinstall


run ln -s /opt/python27-ucs2/bin/python2.7 /usr/bin/python2.7-ucs2 && \
    ln -s /opt/python27-ucs2/bin/pip /usr/bin/pip2.7-ucs2
run wget https://bootstrap.pypa.io/get-pip.py && python2.7-ucs2 get-pip.py
run pip2.7-ucs2 install nose

# Ubuntu only has 2.5.x available prepackaged.
run wget -O/opt/jython-standalone-2.7b3.jar http://search.maven.org/remotecontent?filepath=org/python/jython-standalone/2.7-b3/jython-standalone-2.7-b3.jar
# Wrapper to use nose (etc) from dist-packages
run echo 'export JYTHONPATH=/usr/lib/python2.7/dist-packages; exec java -jar /opt/jython-standalone-2.7b3.jar "$@"' > /usr/bin/jython2.7
run chmod +x /usr/bin/jython2.7

# TODO shrink images by removing deb cache, Python source, etc

# We have a buildbot user, but the Debian thing to do is put it under /var/lib.
# Put it in /home for consistency.
run \
    mkdir /home/buildbot && chown buildbot:users /home/buildbot && \
    usermod --home /home/buildbot buildbot
user buildbot
# $HOME isn't reset by 'user' in the dockerfile, it's still '/'
workdir /home/buildbot
run echo 'python.cachedir=/home/buildbot/.jythoncachedir' > .jython

