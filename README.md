# Debian Packaging for xeus-cling

This is Debian packaging for the
[xeus-cling](https://github.com/jupyter-xeus/xeus-cling) repository with
additional patches that allow it to be built against patched external
[LLVM/Clang](https://salsa.debian.org/dimitry-ishenko/llvm-toolchain-9) and
[Cling](https://salsa.debian.org/dimitry-ishenko/cling) libraries.

xeus-cling along with other supporting packages can be conveniently installed
from either
[ppa:ppa-verse/cling](https://launchpad.net/~ppa-verse/+archive/ubuntu/cling) or
[ppa:ppa-verse/xeus](https://launchpad.net/~ppa-verse/+archive/ubuntu/xeus).

Building instructions:

```bash
p=xeus-cling b=debian # or another branch

# clone this repo
git clone -b ${b} https://salsa.debian.org/dimitry-ishenko/${p}.git
r=$(cd ${p} && git log -n1 --oneline --no-decorate `git describe --tags --abbrev=0` | cut -d/ -f2)
v=${r%-*}
v=${v#*:}

# download upstream tarball
wget https://github.com/jupyter-xeus/xeus-cling/archive/refs/tags/${v}.tar.gz \
    -O ${p}_${v}.orig.tar.gz

# build source package
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
