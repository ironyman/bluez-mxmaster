# Maintainer: Juan Diego Tascon
# Maintainer: Swift Geek
pkgname=bluez-git
_pkgname=bluez
pkgver=5.48.r0.g0d1e3b9c5
pkgrel=1
epoch=1
pkgdesc="Libraries and tools for the Bluetooth protocol stack"

url="http://www.bluez.org/"
arch=('i686' 'x86_64')
license=('GPL2')
depends=('libical' 'libdbus' 'glib2')
optdepends=('cups: CUPS backend')
makedepends=('git')
conflicts=($_pkgname
	   $_pkgname-utils
	   $_pkgname-libs
	   $_pkgname-cups
	   $_pkgname-hid2hci
	   $_pkgname-plugins
	   $_pkgname-hcidump
           'obexd-client'
	   'obexd-server')
provides=($_pkgname
          $_pkgname-utils
	  $_pkgname-libs
	  $_pkgname-cups
	  $_pkgname-hid2hci
	  $_pkgname-plugins)
backup=('etc/bluetooth/main.conf'
	'etc/dbus-1/system.d/bluetooth.conf')
source=("$pkgname::git://git.kernel.org/pub/scm/bluetooth/bluez.git"
	bluetooth.modprobe
	externalhog.patch)
md5sums=('SKIP'
         '671c15e99d7154c2df987b71c5851b3d'
	 'SKIP')

pkgver() {
  cd $pkgname
  git describe --tags --long|sed -r 's,^[^0-9],,;s,([0-9]*-g),r\1,;s,[-_],.,g'
}

prepare() {
  cd $pkgname
  ./bootstrap
}

build() {
  cd $pkgname
  ./configure \
    --prefix=/usr \
    --mandir=/usr/share/man \
    --sysconfdir=/etc \
    --localstatedir=/var \
    --libexecdir=/usr/lib \
    --enable-sixaxis \
    --enable-experimental \
    --enable-manpages \
    --enable-library # this is deprecated
  patch -p1 < $startdir/externalhog.patch
  make
}
  
package() {
  cd $pkgname
  make DESTDIR="${pkgdir}" install
  install -Dm644 src/main.conf        "${pkgdir}/etc/bluetooth/main.conf"
  install -Dm644 ${srcdir}/bluetooth.modprobe "${pkgdir}/usr/lib/modprobe.d/bluetooth-usb.conf"
}
