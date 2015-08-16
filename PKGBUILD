pkgname=mingw-w64-csfml
pkgver=2.1
pkgrel=2
pkgdesc="C bindings for sfml (mingw-w64)"
arch=(any)
url="http://www.sfml-dev.org"
license=("zlib")
makedepends=(mingw-w64-gcc cmake mingw-w64-cmake sfml)
depends=(mingw-w64-crt "mingw-w64-sfml>=$pkgver")
options=(staticlibs !strip !buildflags)
source=("https://github.com/LaurentGomila/CSFML/archive/$pkgver.tar.gz" 
"https://github.com/LaurentGomila/CSFML/commit/d6849a3d57e41b8c9110c51686baae8fee876f82.patch")
md5sums=('960085220e6978c720a62b02cc4a2ec4'
         '2e5f0499d4667ee14c71f9d7d66edd75')

_architectures="i686-w64-mingw32 x86_64-w64-mingw32"

build() {
  cd "$srcdir/CSFML-$pkgver"
  unset LDFLAGS
  for _arch in ${_architectures}; do
    mkdir -p "build-${_arch}" && pushd "build-${_arch}"
    ${_arch}-cmake \
      -DSFML_SYSTEM_LIBRARY_RELEASE=/usr/${_arch}/lib/libsfml-system.a \
      -DSFML_WINDOW_LIBRARY_RELEASE=/usr/${_arch}/lib/libsfml-window.a \
      -DSFML_NETWORK_LIBRARY_RELEASE=/usr/${_arch}/lib/libsfml-network.a \
      -DSFML_GRAPHICS_LIBRARY_RELEASE=/usr/${_arch}/lib/libsfml-graphics.a \
      -DSFML_AUDIO_LIBRARY_RELEASE=/usr/${_arch}/lib/libsfml-audio.a \
      ..
    make
    popd
  done
}

prepare() {
  cd "$srcdir/CSFML-$pkgver"
  patch -p1 -i ../d6849a3d57e41b8c9110c51686baae8fee876f82.patch

}

package() {
  for _arch in ${_architectures}; do
    cd "$srcdir/CSFML-$pkgver/build-$_arch"
    make DESTDIR="$pkgdir" install
    find "$pkgdir/usr/${_arch}" -name '*.dll' | xargs -rtl1 ${_arch}-strip --strip-unneeded
    find "$pkgdir/usr/${_arch}" -name '*.a' -o -name '*.dll' | xargs -rtl1 ${_arch}-strip -g
    find "$pkgdir/usr/${_arch}" -name '*.txt' | xargs -rtl1 rm
  done
}
