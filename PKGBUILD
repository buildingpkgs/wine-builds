pkgname=wine-staging
pkgver=9.18
pkgrel=1
pkgdesc="A compatibility layer for running Windows programs - Staging branch"
arch=('x86_64')
url="https://www.winehq.org/"
license=('LGPL')
provides=(
    "wine"
    "wine-wow64"
    "wine-staging"
    "wine-staging-wow64"
    )
conflicts=('wine' 
           'wine-wow64'
           'wine-staging'
           'wine-esync'
           'wine-tkg-staging-git'
           'wine-tkg-staging-fsync-git'
           'wine-tkg-staging-esync-git'
           "wine-staging-wow64"
           )
replaces=('wine'
          'wine-wow64'
          'wine-staging'
          'wine-esync'
          'wine-tkg-staging-git'
          'wine-tkg-staging-fsync-git'
          'wine-tkg-staging-esync-git'
          "wine-staging-wow64"
          )
options=(!debug)
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
        "lto.patch"
        )
sha256sums=('SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            )

pkgver() {
    git -C $srcdir/wine describe --long --tags | sed 's/\([^-]*-g\)/r\1/;s/-/./g;s/^wine.//;s/^v//;s/\.rc/rc/'
}

build() {
  # uncomment if build fails
  #git -C $srcdir/wine checkout $(cat $srcdir/wine-staging/staging/upstream-commit)
  msg2 "Applying patches..."
  
  patch -Np1 -d $srcdir/wine < ffmpeg.patch
  patch -Np1 -d $srcdir/wine < lto.patch
  
  $srcdir/wine-staging/staging/patchinstall.py --all -W ntdll-Syscall_Emulation DESTDIR="$srcdir/wine"
  
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
