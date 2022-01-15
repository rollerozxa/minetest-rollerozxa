# Maintainer:  ROllerozxa <temporaryemail4meh [gee mail]>
pkgname=minetest-rollerozxa
_pkgname=minetest
pkgver=5.4.0.r501.g7c93b2d7a
pkgrel=1
pkgdesc='Voxel-based sandbox game engine (ROllerozxa''s personal Minetest package with patches)'
url='https://www.minetest.net/'
license=('GPL')
arch=('i686' 'x86_64')
makedepends=('git' 'cmake' 'ninja')
depends=('bzip2' 'libpng' 'libjpeg' 'mesa' 'sqlite' 'openal' 'libvorbis' 'curl'
		'freetype2' 'luajit' 'leveldb' 'gettext' 'hiredis' 'spatialindex' 'gmp' 'discord-rpc-api')
source=('git+https://github.com/minetest/'minetest{,_game}.git
		'git+https://github.com/minetest/irrlicht.git')
sha1sums=(SKIP SKIP SKIP)

conflicts=("${_pkgname}"{,-common,-server})
provides=("${_pkgname}"{,-common,-server})

# patches
_patches=('better_default_resolution' 'faster_gamebar_pagination' 'minetest_discord_rpc')
for i in "${_patches[@]}"; do
	source+=($i'.patch')
	sha1sums+=(SKIP)
done

pkgver() {
	git -C "${_pkgname}" describe --long --tags | sed 's/\([^-]*-g\)/r\1/;s/-/./g'
}

prepare() {
	cd minetest
	
	for i in "${_patches[@]}"; do
		patch -p1 -i '../'$i'.patch'
	done
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
