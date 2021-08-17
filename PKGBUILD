# Maintainer:  ROllerozxa <temporaryemail4meh [gee mail]>
pkgname=minetest-rollerozxa
_pkgname=minetest
pkgver=5.4.0.r282.gb3b075ea0
pkgrel=1
pkgdesc='Voxel-based sandbox game engine (ROllerozxa''s personal Minetest package with patches)'
url='https://www.minetest.net/'
license=('GPL')
arch=('i686' 'x86_64')
makedepends=('git' 'cmake' 'ninja')
depends=('bzip2' 'libpng' 'libjpeg' 'mesa' 'sqlite' 'openal' 'libvorbis' 'curl'
		'freetype2' 'luajit' 'leveldb' 'gettext' 'hiredis' 'spatialindex' 'gmp' 'discord-rpc-api')
source=('git+https://github.com/minetest/'minetest{,_game}.git
		'git+https://github.com/minetest/irrlicht.git'
		'minetest_discord_rpc.patch'
		'discord.cpp'
		'discord.h')
sha1sums=('SKIP' 'SKIP' 'SKIP' 'SKIP' 'SKIP' 'SKIP')

conflicts=("${_pkgname}"{,-common,-server})
provides=("${_pkgname}"{,-common,-server})
install=install

pkgver() {
	git -C "${_pkgname}" describe --long --tags | sed 's/\([^-]*-g\)/r\1/;s/-/./g'
}

prepare() {
	cd minetest
	patch -p1 -i "$srcdir/minetest_discord_rpc.patch"

	cp "$srcdir/discord.cpp" "src/client/"
	cp "$srcdir/discord.h" "src/client/"
}

build() {
	ln -sf "${srcdir}/irrlicht/" "${srcdir}/${_pkgname}/lib/irrlichtmt"

	cd "${srcdir}/${_pkgname}"
	cmake -G Ninja . \
		-DCMAKE_INSTALL_PREFIX=/usr \
		-DENABLE_LEVELDB=1 \
		-DENABLE_POSTGRESQL=0 \
		-DENABLE_SPATIAL=1 \
		-DENABLE_REDIS=1 \
		-DENABLE_LUAJIT=1
	ninja
}

package() {
	cd "${srcdir}/${_pkgname}"
	DESTDIR="${pkgdir}" ninja install
	cp -a "${srcdir}/minetest_game/" "${pkgdir}/usr/share/minetest/games/"
}
