# CAAS-CPUPOOLER

## To Setup Machine
With a non-root user
```
git clone git@github.com:cachengo/rec-manifest.git
git clone git@github.com:cachengo/rec-build-tools.git
git clone git@github.com:cachengo/rec-rpmbuilder.git
git clone git@github.com:cachengo/rec-caas-cpupooler.git

yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
yum install mock createrepo rpmdevtools
getent group mock || groupadd mock
usermod -a -G mock $USER
```

## To build
```
rec-build-tools/build_rpms.sh -m rec-manifest -r rec-rpmbuilder -w work rec-caas-cpupooler
```
You can then make sure the RPMs were created by finding them with `ls work/results/repo` or more generally with `find work`