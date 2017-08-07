# Maintainer: Eric Vidal <eric@obarun.org>

_pkgname=ConsoleKit2
pkgname=consolekit2
pkgver=1.2.0
pkgrel=1
pkgdesc="A framework for defining and tracking users, login sessions, and seats"
arch=(x86_64)
url="https://github.com/ConsoleKit2/ConsoleKit2"
license=('GPL')
provides=('consolekit' 'consolekit2')
replaces=('consolekit')
conflicts=('consolekit')
depends=('dbus' 'glib2' 'libx11' 'polkit-consolekit' 'udev' 'zlib' 'cgmanager')
optdepends=('consolekit-openrc: consolekit openrc initscript'
			'pam: pam modules support'
			'pm-utils: support for suspend/hibernate'
			'consolekit-runitserv: service for runit')
makedepends=('xmlto' 'docbook-xsl')
options=('libtool')
source=("$url/releases/download/$pkgver/$pkgname-$pkgver.tar.bz2"
		'25-consolekit.rules'
		'consolekit.pamd')
sha256sums=('d6ea13b306557a76519388de39bf7f1a1ea9010af147fad4fb3131ce634bd8b3'
            'c5159d9fe8fdd52ad0d6a84af7ba00bac09edaae965896ab0d099a4df1c5ea6b'
            'f7b88e87f447e2d37c12886f57d932c385f19a8fef238e0f1de7a1746d8be69e')
validpgpkeys=('6DD4217456569BA711566AC7F06E8FDE7B45DAAC') # Eric Vidal

prepare(){
	cd $srcdir/$_pkgname-$pkgver
}

build(){
	cd $srcdir/$_pkgname-$pkgver

	./configure  \
		--prefix=/usr \
		--sysconfdir=/etc \
		--sbindir=/usr/bin \
		--with-rundir=/run \
		--libexecdir=/usr/lib/consolekit \
		--localstatedir=/var \
		--enable-polkit \
		--enable-pam-module \
		--enable-udev-acl \
		--enable-docbook-docs \
		--with-dbus-services=/usr/share/dbus-1/services \
		--with-xinitrc-dir=/etc/X11/xinit/xinitrc.d \
		--with-pam-module-dir=/usr/lib/security \
		--without-systemdsystemunitdir

		make
}

package() {
	cd $srcdir/$_pkgname-$pkgver
	make DESTDIR="$pkgdir" install

	rm -rf "${pkgdir}"/run
	
	install -dm 750 "${pkgdir}"/usr/share/polkit-1/rules.d
	install -m 644 ${srcdir}/25-consolekit.rules $pkgdir/usr/share/polkit-1/rules.d/75-consolekit.rules

	install -dm755 $pkgdir/etc/pam.d/
	install -Dm644 ${srcdir}/consolekit.pamd $pkgdir/etc/pam.d/consolekit
}
