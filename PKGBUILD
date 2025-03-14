pkgname=wine-staging-git
pkgrel=1
pkgdesc="A compatibility layer for running Windows programs - Staging branch"
arch=('x86_64')
url="https://www.winehq.org/"
license=('LGPL-2.1-or-later')
provides=(
    wine
    wine-wow64
    wine-staging
    wine-staging-wow64
)
conflicts=(
    wine
    wine-wow64
    wine-staging
    wine-esync
    wine-tkg-staging-git
    wine-tkg-staging-fsync-git
    wine-tkg-staging-esync-git
    wine-staging-wow64
)
replaces=(
    wine
    wine-wow64
    wine-staging
    wine-esync
    wine-tkg-staging-git
    wine-tkg-staging-fsync-git
    wine-tkg-staging-esync-git
    wine-staging-wow64
)
options=(
    !staticlibs 
    !debug 
    !lto
)
depends=(
    wayland
    libxkbcommon
    mesa
    ffmpeg
    sdl2
    libxi
    libxrandr
    gst-plugins-base
    libpcap
    perl
)
optdepends=(
    gst-plugins-good
    gst-plugins-ugly
    gst-plugins-bad
    pcsclite
    opencl-icd-loader
)
makedepends=(
    git
    mingw-w64-gcc
    python
    cups
    sane
    v4l-utils
    libxcomposite
    libxinerama
    gst-plugins-base-libs
    samba
    pcsclite
    opencl-headers
)
source=(
    "git+https://github.com/wine-mirror/wine.git"
    "git+https://github.com/wine-staging/wine-staging.git"
)
sha256sums=(
    SKIP
    SKIP
)

pkgver() {
    git -C "$srcdir/wine" describe --long --tags | sed 's/\([^-]*-g\)/r\1/;s/-/./g;s/^wine.//;s/^v//;s/\.rc/rc/'
}

build() {
  # uncomment if build fails
  #git -C $srcdir/wine checkout $(cat $srcdir/wine-staging/staging/upstream-commit)
  echo "Applying patches..."
  
  "$srcdir/wine-staging/staging/patchinstall.py" --all -W ntdll-Syscall_Emulation DESTDIR="$srcdir/wine"
  
  cd "$srcdir/wine"

  echo "Running configure..."
  ./configure \
  --prefix=/usr \
  --libdir=/usr/lib \
  --enable-archs=x86_64,i386 \
  --with-wayland \
  --without-capi \
  --without-x \
  --without-oss
}

package() {
  cd "$srcdir/wine"

  make prefix="$pkgdir/usr" libdir="$pkgdir/usr/lib" dlldir="$pkgdir/usr/lib/wine" install
  
  i686-w64-mingw32-strip --strip-unneeded "$pkgdir"/usr/lib/wine/i386-windows/*.{dll,exe}
  x86_64-w64-mingw32-strip --strip-unneeded "$pkgdir"/usr/lib/wine/x86_64-windows/*.{dll,exe}
}
