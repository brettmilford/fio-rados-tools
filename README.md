# fio rados tools

A collection of tools for use with [fio](https://github.com/axboe/fio) rados engine.

`fio-3.7-nocleanup.patch` prevents rados from cleaning up objects after a run.

Tested on `4.18.16-300.fc29.x86_64` with `fio-3.7`

Steps to reuse this patch

```shell
dnf download --source fio
rpm -ivh fio-3.7-2.fc29.src.rpm
git clone ... && cd ...
stow nocleanup-patch
rpmbuild -ba ../rpmbuild/SPECS/fio.spec
sudo dnf install ../rpmbuild/RPMS/x86_64/fio-3.7-2.fc29.x86_64.rpm
```

Steps to reproduce the initial patch

```shell
dnf download --source fio
rpm -ivh fio-3.7-2.fc29.src.rpm
cd rpmbuild/SOURCES && tar -xf fio-3.7.tar.bz2
cp -r fio-3.7 fio-3.7p
#edit source in p
diff -uNr fio-3.7/ fio-3.7p/ > ../SOURCES/fio-4.7-nocleanup.patch
rm -r fio-3.7 fio-3.7p
# modify ../SPECS/fio.spec
...
Patch0: fio-3.7-nocleanup.patch
...
%setup ...
%patch0 -p1

# Rebuild package & install
rpmbuild -ba SPECS/fio.spec
sudo dnf install ../RPMS/x86_64/fio-3.7-2.fc29.x86_64.rpm
```
