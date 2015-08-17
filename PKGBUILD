# Maintainer: Thomas Techinus ttech@mostlynothing.info
# Contributor: Romain Schmitz <slopjong .at. slopjong .dot. de>

pkgname=wview
pkgver=5.21.7
pkgrel=1
pkgdesc=" colletion of daemons to provide support to retrieve weather station information."
arch=('i686' 'x86_64')
url="http://www.wviewweather.com/"
license=('GPL')
depends=('zlib' 'libzip' 'libpng' 'gd' 'libusb' 'radlib') # radlib is needed but doesn't work here?
makedepends=('curl' 'gawk' 'wget' 'file' 'sqlite') # we need sqlite to update a column during the packaging
optdepends=('apache2: To view weather station site'
	   'ntp: time sync'
	   'php: web admin interface'
	   'php-gd: For generating web interface images'
	   'php-sqlite: to access weather datababases')
backup=(etc/wview/{graphics,images,images-user,arcrec-header}.conf
	etc/wview/wview-conf.sdb 
	etc/wview/wview-binary
	etc/wview/html/{html-templates,graphics}.conf 
	var/wview/archive/wview-{archive,hilo,history,noaa}.sdb)
options=(!libtool)
source=(http://downloads.sourceforge.net/project/wview/wview/$pkgname-$pkgver/$pkgname-$pkgver.tar.gz wview wview.service)
#install=('wview.install')
sha512sums=('bea1bfe34cf9c63a5a2c45c4e044f2555fe87c29a2018210d260655273e74595e1ad888fab95b4fac2ee99f93faf4e364a76f578b02009b4cf598a9f5c97b409'
	    'd1d115cc82269dac1f05f859a2f288da64636beb2bd546c91f436073df1c309dc68d83fae7169e122f27f065ecdf6a446ad02f6d6017fdb85f079532d05443a6'
	    'ac3b0602b83fadb351496a68ff75afd3ce60352faceee0635e2a5a92cb0999eadddeb2c46b0be320ade87e13865c6d468954d401986e3274bdde2c7011e4a085')
build() {
  cd $srcdir/$pkgname-$pkgver
  ./configure --prefix=/usr --sysconfdir=/etc --localstatedir=/var
  make
}

package() {
  cd $srcdir/$pkgname-$pkgver
  make DESTDIR=$pkgdir install
  echo "update config set value='/var/wview/img' where name='HTMLGEN_IMAGE_PATH';" | sqlite3 "$pkgdir"/etc/wview/wview-conf.sdb
  install -D -m755 "${srcdir}/wview" "${pkgdir}/bin/wviewctl"
  install -D -m644 "${srcdir}/wview.service" "${pkgdir}/usr/lib/systemd/system/wview.service"
  echo "Make sure to run wviewconfig and wviewhtmlconfig if this is your first time installing!"
}
