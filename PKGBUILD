# Maintainer: Eric Vidal <eric@obarun.org>

pkgname=consolekit2
pkgver=1.2.1
pkgrel=1
pkgdesc="A framework for defining and tracking users, login sessions, and seats"
arch=(x86_64)
url="https://github.com/ConsoleKit2/ConsoleKit2"
license=('GPL')
provides=('consolekit' 'consolekit2')
replaces=('consolekit')
conflicts=('consolekit')
depends=('dbus' 'glib2' 'libx11' 'polkit' 'udev' 'zlib' 'cgmanager')
optdepends=('pm-utils: support for suspend/hibernate')
makedepends=('xmlto' 'docbook-xsl' 'gtk-doc' 'pam')
options=('libtool')
_commit=735ae642100c8fef95826186aa2ef749c015cc53  # tag 1.2.1 
source=("$pkgname::git+https://github.com/ConsoleKit2/ConsoleKit2#commit=$_commit"
		'25-consolekit.rules'
		'consolekit.pamd'
		'pam-foreground-compat.ck'
		'tmpfiles.conf')
sha256sums=('SKIP'
            'c5159d9fe8fdd52ad0d6a84af7ba00bac09edaae965896ab0d099a4df1c5ea6b'
            'f7b88e87f447e2d37c12886f57d932c385f19a8fef238e0f1de7a1746d8be69e'
            '7a727be018e26eb7b67df02d3dc493eaa3dafea412801d6bf4b55b4a83c9df7c'
            '778552dc12b3c235bde200e476d4262da0c135f3f6f8b3e975a87881d1f154d1')
validpgpkeys=('6DD4217456569BA711566AC7F06E8FDE7B45DAAC') # Eric Vidal

pkgver() {
	cd "${pkgname}"
	
	git describe --tags | sed -e 's:-:+:g;'
}
#prepare() {
#	cd $srcdir/$pkgname
#	
#	./autogen.sh
#}
build(){
	cd $srcdir/$pkgname

	./autogen.sh  \
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
		--with-dbus-sys=/usr/share/dbus-1/system.d \
		--with-xinitrc-dir=/etc/X11/xinit/xinitrc.d \
		--with-pam-module-dir=/usr/lib/security \
		--without-systemdsystemunitdir \
		--enable-libcgmanager \
		--enable-libevdev \
		--enable-libudev

		make
}

package() {
	cd $srcdir/$pkgname
	make DESTDIR="$pkgdir" install

	#rm -rf "${pkgdir}"/run
	
	install -dm 750 "${pkgdir}"/usr/share/polkit-1/rules.d
	install -m 644 ${srcdir}/25-consolekit.rules $pkgdir/usr/share/polkit-1/rules.d/75-consolekit.rules

	install -dm755 $pkgdir/etc/pam.d/
	install -Dm644 ${srcdir}/consolekit.pamd $pkgdir/etc/pam.d/consolekit
	
	# rename 90-consolekit to .sh
	# xinitrc only load file ending with .sh extension
	mv  "${pkgdir}"/etc/X11/xinit/xinitrc.d/90-consolekit "${pkgdir}"/etc/X11/xinit/xinitrc.d/90-consolekit.sh
	sed -i "s:STARTUP.*:STARTUP=\"\$CK_LAUNCH_SESSION \$@\":g" "${pkgdir}"/etc/X11/xinit/xinitrc.d/90-consolekit.sh
	
	install -Dm 0755 "${srcdir}"/pam-foreground-compat.ck "${pkgdir}"/usr/lib/ConsoleKit/run-session.d/pam-foreground-compat.ck
	
	install -Dm 0644 "${srcdir}"/tmpfiles.conf "${pkgdir}"/usr/lib/tmpfiles.d/consolekit.conf
	
	rm -r "${pkgdir}"/run
}
