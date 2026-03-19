# Maintainer: Momin <your-email@example.com>

_pkgname="hyprsunset"
pkgname="$_pkgname-git"
pkgver=0.3.1.r0.gd0da08a
pkgrel=1
pkgdesc="An application to enable a blue-light filter on Hyprland (fork with sunrise/sunset scheduling)"
arch=('x86_64' 'aarch64')
url="https://github.com/BaigHack3rss/hyprsunset"
license=('BSD-3-Clause')
depends=(
    hyprlang-git
    'hyprutils-git>=0.2.3'
    wayland
    wayland-protocols
)
makedepends=(
    'hyprland-protocols-git>=0.4.0'
    'hyprwayland-scanner-git>=0.4.0'
    cmake
    git
    ninja
)

provides=("$_pkgname=${pkgver%%.r*}")
conflicts=("$_pkgname")

_pkgsrc=$_pkgname
source=("$_pkgsrc::git+$url.git")
sha256sums=('SKIP')

pkgver() {
    cd "$_pkgsrc"
    # Use upstream tags if available, otherwise use commit count and hash
    if git describe --long --tags --abbrev=7 2>/dev/null; then
        git describe --long --tags --abbrev=7 | sed 's/^v//;s/\([^-]*-g\)/r\1/;s/-/./g'
    else
        printf "0.r%s.g%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short=7 HEAD)"
    fi
}

build() {
    local cmake_options=(
        -B build
        -S "$_pkgsrc"
        -G Ninja
        -W no-dev
        -D CMAKE_BUILD_TYPE=None
        -D CMAKE_INSTALL_PREFIX=/usr
    )
    cmake "${cmake_options[@]}"
    cmake --build build
}

package() {
    DESTDIR="$pkgdir" cmake --install build
    install -Dm644 "$_pkgsrc/LICENSE" -t "$pkgdir/usr/share/licenses/$pkgname/"
}
