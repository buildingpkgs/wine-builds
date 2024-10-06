pkgname=wine-staging-wow64
pkgver=9.18
pkgrel=1
pkgdesc="WINE with staging patches applied"
arch=('x86_64')
url="https://www.winehq.org/"
license=('LGPL')
provides=(
    "wine"
    "wine-wow64"
    "wine-staging"
    )
conflicts=('wine' 'wine-wow64' 'wine-staging' 'wine-esync' 'wine-tkg-staging-git' 'wine-tkg-staging-fsync-git' 'wine-tkg-staging-esync-git')
replaces=('wine' 'wine-wow64' 'wine-staging' 'wine-esync' 'wine-tkg-staging-git' 'wine-tkg-staging-fsync-git' 'wine-tkg-staging-esync-git')
options=(staticlibs !lto !debug)
depends=('wayland' 'libxkbcommon' 'mesa' 'ffmpeg' 'sdl2' 'libxi' 'libxrandr')
makedepends=('git' 'mingw-w64-gcc' 'python' 'cups' 'sane' 'v4l-utils' 'libxcomposite' 'libxinerama')
source=("git+https://github.com/wine-mirror/wine.git"
        "git+https://github.com/wine-staging/wine-staging.git"
        "ffmpeg.patch")
sha256sums=('SKIP' 'SKIP' 'SKIP')

build() {
  cd "$srcdir/wine"
  #git checkout $(cat $srcdir/wine-staging/staging/upstream-commit)
  cd ..
  cd ..
  patch -Np1 -d $srcdir/wine < ffmpeg.patch
  msg2 "Applying staging patches..."
  cd "$srcdir/wine-staging"
  git checkout 3695e096530ed650ce08207f8193af028ec85242
  ./staging/patchinstall.py --all -W ntdll-Syscall_Emulation gdi32-rotation DESTDIR="$srcdir/wine"
  
  export CFLAGS="$CFLAGS -ffat-lto-objects"
  cd "$srcdir/wine"

  msg2 "Running configure..."
  ./configure \
  --prefix=/usr \
  --libdir=/usr/lib \
  --with-wayland \
  --enable-archs=x86_64,i386
}

package() {
  cd "$srcdir/wine"

  make prefix="$pkgdir/usr" libdir="$pkgdir/usr/lib" dlldir="$pkgdir/usr/lib/wine" install
}
