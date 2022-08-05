# Maintainer: Vladislav Nepogodin <nepogodin.vlad@gmail.com>
# Contributer : Philip Müller <philm[at]manjaro[dog]org>
# Contributer : Roland Singer <roland[at]manjaro[dog]org>

pkgbase=mhwd-xerolinux
pkgname=('mhwd-xerolinux' 'mhwd-db-xerolinux')
pkgname=mhwd-xerolinux
pkgver=0.6.11
pkgrel=1
pkgdesc="Manjaro's Hardware Detection for XeroLinux"
arch=(x86_64)
url="https://github.com/xerolinux/mhwd-xerolinux"
license=(GPLv3)
depends=('gcc-libs' 'hwinfo')
makedepends=('cmake' 'ninja' 'git')
groups=('xerolinux')
_commit_hash=7f1aa8217f4bb4adc9439675e6400f8921c9b44b
source=("git+${url}.git#commit=${_commit_hash}"
        "git+${url/-xerolinux}-db-xerolinux.git")
sha256sums=('SKIP'
            'SKIP')
options=(!strip)

build() {
  cd $pkgname

  _cpuCount=$(grep -c -w ^processor /proc/cpuinfo)

  cmake -S . -Bbuild \
        -GNinja \
        -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_INSTALL_PREFIX=/usr \
        -DCMAKE_INSTALL_LIBDIR=lib
  cmake --build build --parallel $_cpuCount
}

package_mhwd-xerolinux() {
  pkgdesc="mhwd-xerolinux library and application"
  depends=('hwinfo' 'mesa' 'mhwd-db-xerolinux' 'pacman')
  optdepends=('lib32-mesa: for 32bit libgl support')
  provides=('mhwd' 'mhwd-xerolinux')
  conflicts=('mhwd' 'mhwd-xerolinux')
  install=mhwd.install

  cd $pkgname/build

  DESTDIR="${pkgdir}" ninja install
  install -d -m755 "$pkgdir"/var/lib/mhwd/{db,local}/{pci,usb}
}

package_mhwd-db-xerolinux() {
  pkgdesc="mhwd-xerolinux Database"
  depends=('libnotify' 'mhwd-nvidia' 'mhwd-nvidia-390xx' 'mhwd-nvidia-470xx' 'mhwd-amdgpu' 'mhwd-ati' 'mhwd-nvidia>=455')
  provides=('mhwd-db' 'mhwd-db-xerolinux')
  conflicts=('mhwd-db' 'mhwd-db-xerolinux')
  install=mhwd-db.install

  cd $pkgname

  mkdir -p "$pkgdir"/var/lib/mhwd/db
  mkdir -p "$pkgdir"/etc/X11/mhwd.d
  cp -r pci "$pkgdir"/var/lib/mhwd/db/
}

# vim:set sw=2 sts=2 et:
