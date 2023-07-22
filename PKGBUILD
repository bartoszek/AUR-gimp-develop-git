#!/hint/bash
# Maintainer : bartus <arch-user-repoᘓbartus.33mail.com>
# Contributor: Filipe Laíns (FFY00) <filipe.lains@gmail.com>
# Contributor: Iru Cai <mytbk920423@gmail.com>
# Contributor: Alexander Hunziker <alex.hunziker@gmail.com>
# Contributor: Alessio Biancalana <dottorblaster@gmail.com>

# Configuration
_fragment="${FRAGMENT:-#branch=master}"

pkgname=gimp-develop-git
_pkgname=${pkgname%-develop-git}
epoch=1
pkgver=2.99.16.r103.gba5b4794e1
pkgrel=1
pkgdesc="GNU Image Manipulation Program (non-conflicting git version)"
arch=('i686' 'x86_64')
url="https://www.gimp.org"
license=('GPL' 'LGPL')
depends=(
  'appstream-glib'
  'babl'
  'cairo'
  'dbus-glib'
  'desktop-file-utils'
  'enchant'
  'gegl'
  'gobject-introspection'
  'gtk-doc'
  'icu'
  'lcms2'
  'libart-lgpl>=2.3.19'
  'libexif>=0.6.15'
  'libgexiv2'
  'librsvg'
  'libwmf'
  'mypaint-brushes1'
  'openexr'
  'poppler-data'
  'poppler-glib'
)
makedepends=(
  'aalib'
  'alsa-lib'
  'alsa-lib>=1.0.0'
  'curl'
  'ghostscript'
  'git'
  'gjs'
  'glib-networking'
  'intltool>=0.40.1'
  'iso-codes'
  'libheif'
  'libmng'
  'libwebp'
  'libxpm'
  'libxslt'
  'luajit'
  'meson'
  'python-gobject'
  'webkit2gtk'
  'xorg-server-xvfb'
  'zlib'
)
checkdepends=('xorg-server-xvfb')
optdepends=(
  'aalib: ASCII art support'
  'alsa-lib: for MIDI event controller module'
  'curl: for URI support'
  'ghostscript: for postscript support'
  'gjs: JavaScript scripting support'
  'gutenprint: for sophisticated printing only as gimp has built-in cups print support'
  'iso-codes: Language support'
  'libheif: HEIF support'
  'libmng: MNG support'
  'libwebp: WebP support'
  'libxpm: XPM support'
  'luajit: LUA scripting support'
  'webkit2gtk: HTML renderer and web content engine'
  'zlib: Compression routines'
)
source=(
  "git+https://gitlab.gnome.org/GNOME/gimp.git${_fragment}"
  'linux.gpl'
)
sha512sums=(
  'SKIP'
  '6f33d57f242fa8ce04b65e06a712bd54677306a45b22cb853fbe348089cd4673bd4ed91073074fe067166fe8951c370f8bbbc386783e3ed5170d52e9062666fe'
)

pkgver() {
  cd "$srcdir/gimp"

  git describe --long --tags | sed 's/^GIMP_//;s/_/./g;s/\([^-]*-g\)/r\1/;s/-/./g'
}

prepare() {
  export CFLAGS CXXFLAGS LDFLAGS
  meson "${srcdir}/${_pkgname}"\
        "${srcdir}/build"\
        --prefix=/usr
}

build() {
  export NINJA_STATUS="[%p | %f<%r<%u | %cbps ] "
# shellcheck disable=SC2046 # allow MAKEFLAGS to split when passing multiple flags.
 ninja $(grep -oP -- '-+[A-z]+ ?[0-9]*'<<<"${MAKEFLAGS:--j1}") -C "${srcdir}/build"
}

package() {
  DESTDIR="${pkgdir}" ninja -C "${srcdir}/build" install
  install -Dm 644 "${srcdir}/linux.gpl" "${pkgdir}/usr/share/gimp/2.99/palettes/Linux.gpl"

  #fix gimp.desktop
  mv "${pkgdir}"/usr/share/applications/gimp.desktop "${pkgdir}"/usr/share/applications/gimp-2.99.desktop
  sed -i 's/Icon=gimp/&-2.99/' "${pkgdir}"/usr/share/applications/gimp-2.99.desktop

  #fix icons
  for icon in $(find "${pkgdir}"/usr/share/icons/ -type f); do
    mv "${icon}" "${icon%.*}-2.99.${icon##*.}"
  done

  #fix metainfo
  rm -rf "${pkgdir}"/usr/share/metainfo
}
