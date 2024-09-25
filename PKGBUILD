pkgname=wine-staging-wow64
pkgver=9.18
pkgrel=1
pkgdesc="WINE with staging patches applied"
arch=('x86_64')
url="https://www.winehq.org/"
license=('LGPL')
options=(staticlibs !lto !debug)
depends=('wayland' 'libxkbcommon' 'mesa' 'ffmpeg' 'sdl2' 'libxi' 'libxrandr')
makedepends=('git' 'mingw-w64-gcc' 'python' 'cups' 'sane' 'pipewire-pulse' 'v4l-utils' 'libxcomposite' 'libxinerama')
source=("git+https://github.com/wine-mirror/wine.git"
        "git+https://github.com/wine-staging/wine-staging.git")
sha256sums=('SKIP' 'SKIP')

build() {

  cd "$srcdir/wine"

  msg2 "Applying staging patches..."
  cd "$srcdir/wine-staging"

  ./staging/patchinstall.py --all DESTDIR="$srcdir/wine"

  cd "$srcdir/wine"

  echo "Running configure..."
  ./configure --prefix=/usr --libdir=/usr/lib --with-wayland --enable-archs=x86_64,i386
}

package() {
  cd "$srcdir/wine"

  make prefix="$pkgdir/usr" libdir="$pkgdir/usr/lib" dlldir="$pkgdir/usr/lib/wine" install
}
