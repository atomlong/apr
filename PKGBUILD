# Maintainer: Alexey Pavlov <alexpux@gmail.com>

pkgname=('apr' 'apr-devel')
pkgver=1.7.4
pkgrel=2
pkgdesc="The Apache Portable Runtime"
arch=('i686' 'x86_64')
url="https://apr.apache.org/"
makedepends=('libcrypt-devel' 'libuuid-devel' 'autotools' 'gcc')
options=('!libtool')
license=('spdx:Apache-2.0')
source=(https://archive.apache.org/dist/apr/apr-${pkgver}.tar.bz2
        enable-threads.patch
        no-undefined.patch)
sha256sums=('fc648de983f3a2a6c9e78dea1f180639bd2fad6c06d556d4367a701fe5c35577'
            '0b7d8f13bb2621fea5fa45460a69959811ba9116c3ee04632a1e5ab9e5dbdf2c'
            '3f3079ceef8338ea3bdb20a9fb6852787be579efcf9974097be996a1a581b6e0')

prepare() {
  cd "${srcdir}/apr-${pkgver}"
  
  patch -p1 -i ${srcdir}/enable-threads.patch
  patch -p1 -i ${srcdir}/no-undefined.patch
  autoreconf -fi
}

build() {
  cd "${srcdir}/apr-${pkgver}"

  export MSYSTEM=CYGWIN
  local CYGWIN_CHOST="${CHOST/-msys/-cygwin}"
  ./configure \
      --build=${CYGWIN_CHOST} \
      --prefix=/usr \
      --libexecdir=/usr/lib/apr/modules \
      --datadir=/usr/share/apr \
      ac_cv_header_windows_h=no \
      ac_cv_header_winsock2_h=no

  make
  make DESTDIR="${srcdir}/dest" install
}

check() {
  cd "${srcdir}/apr-${pkgver}"
  make -j1 check || true
}

package_apr() {
  depends=('libcrypt' 'libuuid')
  groups=('libraries')

  mkdir -p ${pkgdir}/usr/{bin,share}
  cp -f ${srcdir}/dest/usr/bin/*.dll ${pkgdir}/usr/bin/
}

package_apr-devel() {
  pkgdesc="Libapr headers and libraries"
  groups=('development')
  depends=("apr=${pkgver}" "libcrypt-devel" "libuuid-devel")
  options=('staticlibs')

  mkdir -p ${pkgdir}/usr/{bin,share}
  cp -f ${srcdir}/dest/usr/bin/*-config ${pkgdir}/usr/bin/
  cp -rf ${srcdir}/dest/usr/include ${pkgdir}/usr/
  cp -rf ${srcdir}/dest/usr/lib ${pkgdir}/usr/
  cp -rf ${srcdir}/dest/usr/share/apr ${pkgdir}/usr/share/
}
