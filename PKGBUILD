# Maintainer: Gustavo Alvarez <sl1pkn07@gmail.com>
# Contributor: Ionut Biru <ibiru@archlinux.org>

pkgname=libva-vdpau-driver-chromium
pkgver=0.7.4
pkgrel=6
pkgdesc="VDPAU backend for VA API. (special version for chromium)"
arch=('x86_64')
url='http://freedesktop.org/wiki/Software/vaapi'
license=('GPL')
depends=('libva'
         'libvdpau'
         'libgl'
         )
makedepends=('mesa'
             'xorgproto'
             )
replaces=('vdpau-video')
provides=('libva-vdpau-driver')
conflicts=('libva-vdpau-driver')
source=("https://freedesktop.org/software/vaapi/releases/libva-vdpau-driver/libva-vdpau-driver-${pkgver}.tar.bz2"
        'libva-vdpau-driver-0.7.4-glext-missing-definition.patch::https://git.archlinux.org/svntogit/packages.git/plain/trunk/libva-vdpau-driver-0.7.4-glext-missing-definition.patch?h=packages/libva-vdpau-driver'
        'libva-vdpau-driver-0.7.4-libvdpau-0.8.patch::https://git.archlinux.org/svntogit/packages.git/plain/trunk/libva-vdpau-driver-0.7.4-libvdpau-0.8.patch?h=packages/libva-vdpau-driver'
        'libva-vdpau-driver-0.7.4-VAEncH264VUIBufferType.patch::https://git.archlinux.org/svntogit/packages.git/plain/trunk/libva-vdpau-driver-0.7.4-VAEncH264VUIBufferType.patch?h=packages/libva-vdpau-driver'
        'sigfpe-crash.patch::https://bugs.freedesktop.org/attachment.cgi?id=142296'
        'fallback-x.patch'
        'implement-vaquerysurfaceattributes.patch'
        'vdpau-driver-template.patch'
        )
sha256sums=('155c1982f0ac3f5435ba20b221bcaa11be212c37db548cd1f2a030ffa17e9bb9'
            '776bfe4c101cdde396d8783029b288c6cd825d0cdbc782ca3d94a5f9ffb4558c'
            '5e567b026b97dc0e207b6c05410cc1b7b77a58ceb5046801d0ea1a321cba3b9d'
            '1ae32b8e5cca1717be4a63f09e8c6bd84a3e9b712b933816cdb32bb315dbda98'
            'ef21397b0ed55f170e57fa1afef42b8b491491149af09b151f97ef3cc20ed0b7'
            '245dab80e14d7f302c1367ce59c96874b5fc40d253a789c0199130ae99c43599'
            'a91e11d2826715b29c2a08036f0e70ed45df7139987ff37c84b5c04c4a9c132f'
            'SKIP'
            )

prepare() {
  mkdir -p build

  patch -d "libva-vdpau-driver-${pkgver}" -p1 -i "${srcdir}/libva-vdpau-driver-0.7.4-glext-missing-definition.patch"
  patch -d "libva-vdpau-driver-${pkgver}" -p1 -i "${srcdir}/libva-vdpau-driver-0.7.4-libvdpau-0.8.patch"
  patch -d "libva-vdpau-driver-${pkgver}" -p1 -i "${srcdir}/libva-vdpau-driver-0.7.4-VAEncH264VUIBufferType.patch"
  patch -d "libva-vdpau-driver-${pkgver}" -p1 -i "${srcdir}/fallback-x.patch"
  patch -d "libva-vdpau-driver-${pkgver}" -p1 -i "${srcdir}/sigfpe-crash.patch"
  patch -d "libva-vdpau-driver-${pkgver}" -p1 -i "${srcdir}/implement-vaquerysurfaceattributes.patch"
  patch -d "libva-vdpau-driver-${pkgver}" -p2 -i "${srcdir}/vdpau-driver-template.patch"

  # https://bbs.archlinux.org/viewtopic.php?id=247933
  sed -e '/v4l2_buffer/d' \
      -e '/v4l2_format/d' \
      -i "${srcdir}/libva-vdpau-driver-${pkgver}/src/"vdpau_video.{h,c}

  # https://gitlab.freedesktop.org/lima/mesa/commit/1edaa33135feffe1082bf491e098224121615b08
  sed 's|Baseline|ConstrainedBaseline|g' -i "${srcdir}/libva-vdpau-driver-${pkgver}/src/vdpau_decode.c"
}

build() {
  cd build
  ../"libva-vdpau-driver-${pkgver}"/configure \
    --prefix=/usr

  make
}

package() {
  make -C build DESTDIR="${pkgdir}" install
}

