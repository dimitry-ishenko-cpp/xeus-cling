# Debian Packaging for xeus-cling

This is Debian packaging repository for 
[xeus-cling](https://github.com/jupyter-xeus/xeus-cling).

Precompiled Debian package for xeus-cling can be installed from
[ppa:ppa-verse/xeus-cling](https://launchpad.net/~ppa-verse/+archive/ubuntu/xeus-cling).

Or you can build it yourself:

```bash
p=xeus-cling b=debian # or another branch

# clone this repo
git clone -b ${b} https://salsa.debian.org/dimitry-ishenko/${p}.git
r=$(cd ${p} && git log -n1 --oneline --no-decorate `git describe --tags --abbrev=0` | cut -d/ -f2)
v=${r%-*}
v=${v#*:}

# download and unpack upstream tarball
f=${p}_${v}.orig.tar.gz
wget https://github.com/jupyter-xeus/xeus-cling/archive/refs/tags/${v}.tar.gz -O ${f}

tar -C ${p} --strip-components=1 -xzf ${f}

# create source package
dpkg-source -b ${p}

# build .deb package locally...
sudo pbuilder build ${p}_${r}.dsc

# ...or generate .changes file and upload it to a PPA
# NB: change -sa to -sd when upgrading
(cd ${p} && dpkg-genchanges -S -sa -O../${p}_${r}_amd64.changes)

debsign ${p}_${r}_amd64.changes
dput ppa:... ${p}_${r}_amd64.changes

# share and enjoy
```
