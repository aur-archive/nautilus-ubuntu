# Maintainer: Charles Bos <charlesbos1 AT gmail>
# Contributpr: Xiao-Long Chen <chenxiaolong@cxl.epac.to>
# Contributor: Jan de Groot <jgc@archlinux.org>
# Contributor: thn81 <root@scrat>

# Note: The original package by Xiao-Long Chen split nautilus and libnautilus-extension into two packages. I've had to removed the division because the AUR doesn't currently support split packages.

pkgname=nautilus-ubuntu
_ppa_rel=0ubuntu1~trusty1
pkgver=3.12.0
pkgrel=1
pkgdesc="GNOME and Unity file manager"
arch=('i686' 'x86_64')
license=('GPL')
depends=('libexif' 'gnome-desktop' 'exempi' 'gvfs' 'desktop-file-utils' 'gnome-icon-theme' 'dconf' 'libtracker-sparql' 'libnotify' 'nautilus-sendto' 'libunity' 'libzeitgeist')
makedepends=('intltool' 'gobject-introspection' 'python')
provides=('nautilus' 'libnautilus-extension' 'libnautilus-extension-ubuntu')
groups=('gnome')
conflicts=('nautilus' 'libnautilus-extension')
url="http://www.gnome.org"
options=('!emptydirs')
install=nautilus.install
source=("http://ftp.gnome.org/pub/gnome/sources/nautilus/${pkgver%.*}/nautilus-${pkgver}.tar.xz"
        "http://ppa.launchpad.net/gnome3-team/gnome3-staging/ubuntu/pool/main/n/nautilus/nautilus_${pkgver}-${_ppa_rel}.debian.tar.gz")
sha512sums=('e42eef48564c8b1fd0cb1cadee285be0c8b1e0840527498f4e4e2d25719d31d3dc1c780cd09e521f187ed98502e0b223aaac589ca22bdc6a4e3bd4c3495feb7d'
            '95f31d7f526f83ca53885b3fce1e0960ae9d478511461f9ee115c91031307c65a218ac56c69f90f616a330789d2d64a201bd1c3237fc30ad6947eda064e5f3e1')

prepare() {
  cd "${srcdir}/${pkgname%-*}-${pkgver}"

  # Apply Ubuntu's patches

  # Disable patches
    # Don't use Ubuntu help
      sed -i '/15_use-ubuntu-help.patch/d' "${srcdir}/debian/patches/series"

  for i in $(grep -v '#' "${srcdir}/debian/patches/series"); do
    msg "Applying ${i} ..."
    patch -p1 -i "${srcdir}/debian/patches/${i}"
  done

  sed -i '/gnome_bg_set_draw_background/d' libnautilus-private/nautilus-desktop-background.c
}

build() {
  cd "${srcdir}/${pkgname%-*}-${pkgver}"

  autoreconf -vfi

  ./configure \
    --prefix=/usr \
    --sysconfdir=/etc \
    --localstatedir=/var \
    --disable-static \
    --libexecdir=/usr/lib/nautilus \
    --disable-update-mimedb \
    --disable-packagekit \
    --disable-schemas-compile \
    --disable-appindicator \
    --enable-unity

  make
}

package() {
  cd "${srcdir}/nautilus-${pkgver}"
  make DESTDIR="${pkgdir}/" install

  # Ubuntu specific stuff
  install -dm755 "${pkgdir}/usr/share/pixmaps/"
  install -dm755 "${pkgdir}/usr/share/applications/"
  install -m644 "${srcdir}/debian/nautilus.xpm" "${pkgdir}/usr/share/pixmaps/"
  install -m644 "${srcdir}/debian/mount-archive.desktop" "${pkgdir}/usr/share/applications/"
  install -m644 "${srcdir}/debian/nautilus-folder-handler.desktop" "${pkgdir}/usr/share/applications/"

  # Use language packs
  rm -r "${pkgdir}/usr/share/locale/"
}

# vim:set ts=2 sw=2 et:
