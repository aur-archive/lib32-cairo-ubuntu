# Contributor: Paul Bredbury <brebs@sent.com>
# Contributor: Biru Ionut <biru.ionut at gmail.com>
# Maintainer: Andrea Fagiani <andfagiani {at} gmail {dot} com>

# Installation order:  freetype2-ubuntu fontconfig-ubuntu libxft-ubuntu cairo-ubuntu
_pkgbasename=cairo-ubuntu
pkgname=lib32-$_pkgbasename
pkgver=1.10.2
_ubver=1.10.2-6.1ubuntu2
pkgrel=6
pkgdesc="Cairo vector graphics library, with Ubuntu's LCD rendering patches (32-bit)"
arch=(x86_64)
url="https://launchpad.net/ubuntu/precise/+source/cairo"
license=('LGPL' 'MPL')
depends=('lib32-libpng' 'lib32-libxrender' 'lib32-fontconfig-ubuntu' 'lib32-libxft-ubuntu'
         'lib32-pixman' 'lib32-glib2' $_pkgbasename)
makedepends=('gcc-multilib')
provides=("lib32-cairo=$pkgver")
conflicts=('lib32-cairo')
options=('!libtool')
source=(http://cairographics.org/releases/cairo-$pkgver.tar.gz
        http://archive.ubuntu.com/ubuntu/pool/main/c/cairo/cairo_$_ubver.debian.tar.gz
        cairo-respect-fontconfig.patch)

md5sums=('f101a9e88b783337b20b2e26dfd26d5f'
         'dbb6949571fef3c1d3777dc61c0cea27'
         '79f7c141c49f3d65ab308cc706d50914')

build() {
  export CC="gcc -m32"
  export CXX="g++ -m32"
  export PKG_CONFIG_PATH="/usr/lib32/pkgconfig"

  cd $srcdir/cairo-$pkgver

  for _f in $(cat $srcdir/debian/patches/series) ; do    
    patch -Np1 -i $srcdir/debian/patches/$_f    
  done

  patch -Np1 -i $srcdir/cairo-respect-fontconfig.patch
  
  ./configure --prefix=/usr --libdir=/usr/lib32 \
        --sysconfdir=/etc --localstatedir=/var \
	--disable-static --enable-tee
  make
}

package() {
  cd $srcdir/cairo-$pkgver
  make DESTDIR=$pkgdir install
  rm -rf ${pkgdir}/usr/{bin,include,share}
}
