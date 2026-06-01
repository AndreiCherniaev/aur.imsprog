# Maintainer: Kristopher James Kent <kris@kjkent.dev>
pkgname='imsprog'
_pkgname='IMSProg'
depends=('libusb>=1.0.20' 'qt6-base' 'wget')
makedepends=('cmake>=3.13.0', 'qt6-tools')
url="https://github.com/bigbigmdm/$pkgname"
pkgver='1.8.4'
pkgrel='1'
arch=('x86_64')
license=('GPL-3.0-only')
pkgdesc='I2C, SPI and MicroWire EEPROM/flash chip programmer for CH341A devices.'
source=("$url/archive/refs/tags/v$pkgver.tar.gz")

# Used in lieu of upstream hash
# curl -Ls https://github.com/bigbigmdm/IMSProg/archive/refs/tags/v$pkgver.tar.gz | b2sum | cut -d ' ' -f 1
b2sums=('63b252b58bde56b8b8fe6245debe521b0c9997d9c1a7100275da18a42bbc3f8d0bb741b6b378cc96da6742b9c7491b66e396ef2f6c8fbbe78a93c00450ab7d74')

_srcprefix="$_pkgname-$pkgver/$_pkgname"
_srcdirs=("${_srcprefix}_editor" "${_srcprefix}_programmer")

build() {
  local srcdir

  # Sane defaults for Arch package guidelines. Commented if unused by project.
  # https://wiki.archlinux.org/title/CMake_package_guidelines
  local cmakeopts=(
    -Wno-dev
    -D 'CMAKE_BUILD_TYPE=None'
    #-D 'CMAKE_INSTALL_LIBDIR=lib'
    #-D "CMAKE_INSTALL_LIBEXECDIR=lib/$pkgname"
    -D 'CMAKE_INSTALL_PREFIX=/usr'
    -D 'CMAKE_SKIP_INSTALL_RPATH=YES'
    -D 'CMAKE_SKIP_RPATH=YES'
    #-D 'FETCHCONTENT_FULLY_DISCONNECTED=ON'
  )

  for srcdir in "${_srcdirs[@]}"; do
    local bindir="$srcdir/build"

    cmake "${cmakeopts[@]}" -S "$srcdir" -B "$bindir"
    cmake --build "$bindir" --parallel
  done
}

package() {
  local bindir

  for bindir in "${_srcdirs[@]/%//build}"; do
    DESTDIR="$pkgdir" cmake --install "$bindir"
  done
}
