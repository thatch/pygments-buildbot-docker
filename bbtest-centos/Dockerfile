from centos:latest

run yum install -y python-nose scl-utils mercurial make

# need gcc to build twisted 14... would be nice to have as an rpm though, to
# avoid this bloat.
run yum groupinstall -y "Development tools"

# https://www.softwarecollections.org/en/scls/rhscl/python33/
run yum install -y https://www.softwarecollections.org/en/scls/rhscl/python33/epel-7-x86_64/download/rhscl-python33-epel-7-x86_64.noarch.rpm
# this is a group, includes python33-python-nose
run yum install -y python33

# The normal way to do this is run 'scl enable python33 bash' which gives you a
# new shell with these vars set.  We'll use a wrapper.
# TODOL check that the "$@" quoting is working.
run echo -e '#!/bin/bash\nexport LD_LIBRARY_PATH=/opt/rh/python33/root/usr/lib64\nexec /opt/rh/python33/root/usr/bin/python3.3 "$@"' > /usr/bin/python3.3; chmod +x /usr/bin/python3.3

# TODO: Build this once from source, including twisted,
# per http://trac.buildbot.net/wiki/SourceRPMS
run yum install -y python-devel
run easy_install pip && pip install buildbot-slave

run useradd -m -G users buildbot -s /bin/bash
user buildbot
# $HOME is still / even after ther 'user' command :(
workdir /home/buildbot

