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
        "a608ef1.patch"
        "fsync_futex_waitv.patch"
        "fsync-unix-staging.patch"
        "HACK-user32-Always-call-get_message-request-after-waiting.patch"
        "nostale_mouse_fix.patch"
        "opencl-fixup.patch"
        "Return_nt_filename_and_resolve_DOS_drive_path.patch")
sha256sums=('SKIP' 'SKIP' 'SKIP' 'SKIP' 'SKIP' 'SKIP' 'SKIP' 'SKIP' 'SKIP')

build() {
  patch -Np1 -d $srcdir/wine < Return_nt_filename_and_resolve_DOS_drive_path.patch
  patch -Np1 -d $srcdir/wine < a608ef1.patch
  patch -Np1 -d $srcdir/wine < opencl-fixup.patch
  patch -Np1 -d $srcdir/wine < nostale_mouse_fix.patch
  patch -Np1 -d $srcdir/wine < HACK-user32-Always-call-get_message-request-after-waiting.patch
  cd "$srcdir/wine"
  git checkout $(cat $srcdir/wine-staging/staging/upstream-commit)
  msg2 "Applying staging patches..."
  cd "$srcdir/wine-staging"

  ./staging/patchinstall.py --all -W ntdll-Syscall_Emulation DESTDIR="$srcdir/wine"
  
  
  export CFLAGS="$CFLAGS -ffat-lto-objects"
  cd ..
  cd ..
  patch -Np1 -d $srcdir/wine < fsync-unix-staging.patch
  patch -Np1 -d $srcdir/wine < fsync_futex_waitv.patch
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
