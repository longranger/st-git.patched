# Maintainer: Gaetan Bisson <bisson@archlinux.org>
# Contributor: Scytrin dai Kinthra <scytrin@gmail.com>

pkgname=st-git
_pkgname=st
epoch=1
pkgver=20150205.aba6c29
pkgrel=1
pkgdesc='Simple virtual terminal emulator for X'
url='http://git.suckless.org/st/'
arch=('i686' 'x86_64' 'armv7h')
license=('MIT')
depends=('libxft')
makedepends=('ncurses' 'libxext' 'git')
source=('git://git.suckless.org/st' st-no-bold-colors.patch st-solarized-dark.patch st-config.patch)
sha1sums=('SKIP'
          'ff7db7c22488e0f5da9b5aa1b9310b9c583ca3bc'
          '30a0f2d2a244262f7f931829f711a2f3bd0e0302'
          '145066c89f6c2181f767c6e338c3d50f81addcf7')

provides=("${_pkgname}")
conflicts=("${_pkgname}")

pkgver() {
	cd "${srcdir}/${_pkgname}"
	git log -1 --format='%cd.%h' --date=short | tr -d -
}

prepare() {
	cd "${srcdir}/${_pkgname}"
	sed \
		-e 's/CPPFLAGS =/CPPFLAGS +=/g' \
		-e 's/CFLAGS =/CFLAGS +=/g' \
		-e 's/LDFLAGS =/LDFLAGS +=/g' \
		-i config.mk
	sed '/char font/s/".*"/"Fixed:pixelsize=13:style=SemiCondensed"/' -i config.def.h
	sed '/@tic/d' -i Makefile
	patch -p1 -i $srcdir/st-no-bold-colors.patch
	patch -p1 -i $srcdir/st-solarized-dark.patch
	patch -p1 -i $srcdir/st-config.patch
}

build() {
	cd "${srcdir}/${_pkgname}"
	make X11INC=/usr/include/X11 X11LIB=/usr/lib/X11
}

package() {
	cd "${srcdir}/${_pkgname}"
	make PREFIX=/usr DESTDIR="${pkgdir}" install
	install -Dm644 LICENSE "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
	install -Dm644 README "${pkgdir}/usr/share/doc/${pkgname}/README"
}
