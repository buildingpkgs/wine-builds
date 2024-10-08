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
conflicts=('wine' 
           'wine-wow64'
           'wine-staging'
           'wine-esync'
           'wine-tkg-staging-git'
           'wine-tkg-staging-fsync-git'
           'wine-tkg-staging-esync-git'
           )
replaces=('wine'
          'wine-wow64'
          'wine-staging'
          'wine-esync'
          'wine-tkg-staging-git'
          'wine-tkg-staging-fsync-git'
          'wine-tkg-staging-esync-git'
          )
options=(staticlibs !lto !debug)
depends=('wayland'
         'libxkbcommon'
         'mesa'
         'ffmpeg'
         'sdl2'
         'libxi'
         'libxrandr'
         )
makedepends=('git'
             'mingw-w64-gcc'
             'python'
             'cups'
             'sane'
             'v4l-utils'
             'libxcomposite'
             'libxinerama'
             'gst-plugins-base-libs'
             'samba'
             )
source=("git+https://github.com/wine-mirror/wine.git"
        "git+https://github.com/wine-staging/wine-staging.git"
        "ffmpeg.patch"
        )
sha256sums=('SKIP'
            'SKIP'
            'SKIP'
            )

build() {
  cd "$srcdir/wine"
  git checkout $(cat $srcdir/wine-staging/staging/upstream-commit)
  cd ..
  cd ..
  patch -Np1 -d $srcdir/wine < ffmpeg.patch
  msg2 "Applying staging patches..."
  cd "$srcdir/wine-staging"
  
  ./staging/patchinstall.py --all -W ntdll-Syscall_Emulation DESTDIR="$srcdir/wine"
  
  export CFLAGS="$CFLAGS -ffat-lto-objects"
  cd "$srcdir/wine"

  msg2 "Running configure..."
  ./configure \
  --prefix=/usr \
  --libdir=/usr/lib \
  --with-wayland \
  --enable-archs=x86_64,i386 \
  --without-opencl \
  --without-capi \
  --without-oss \
  --without-pcsclite
}

package() {
  cd "$srcdir/wine"

  make prefix="$pkgdir/usr" libdir="$pkgdir/usr/lib" dlldir="$pkgdir/usr/lib/wine" install
  
  i686-w64-mingw32-strip --strip-unneeded "$pkgdir"/usr/lib/wine/i386-windows/*.dll
  x86_64-w64-mingw32-strip --strip-unneeded "$pkgdir"/usr/lib/wine/x86_64-windows/*.dll
}
