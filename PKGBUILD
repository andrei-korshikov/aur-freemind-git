# Maintainer: Andrei Korshikov <andrej.s.korshikov at gmail dot com>
# Contributor: Andrzej Giniewicz <gginiu at gmail dot com>
# Contributor: Archie <mymaud at gmail dot com>

pkgname='freemind-git'
_release='1.1.0_Beta_2'
pkgver=1.1.0_Beta_2.r1064.cc87d4a4
pkgrel=1
pkgdesc='A mind mapper, and a hierarchical editor with strong emphasis on folding'
arch=('any')
url='https://freemind.sourceforge.io/wiki/'
license=('GPL-2.0-or-later')
# 'cc87d4a4' can run on Java 8 or 11, and can be compiled with Java 8 only
depends=('java-runtime<=11')
makedepends=('apache-ant' 'git' 'java-environment=8' 'libicns')   # 'libicns' provides 'icns2png'
provides=('freemind')
conflicts=('freemind' 'freemind-unstable')  # remove 'freemind-unstable' on 01.01.2025
replaces=('freemind-unstable')  # remove on 01.01.2025
source=(
    'git+https://git.code.sf.net/p/freemind/code'
    'freemind.desktop'
    )
b2sums=(
    'SKIP'
    'c1aeebe028d59c6d3c0f0c7ef5f4353ef08d7f7103944305cc3166d65b9ff593fd7c4eeea7f28e43ad887308fa45c334c91b81f75fc1b4f43f5d8dacb610b0b7'
    )

prepare() {
    cd 'code' || exit
    # 'bb964c15' and '832707b2' can't be compiled
    # '095dc4e6' leads to 'Exception in thread "AWT-EventQueue-0". java.lang.StackOverflowError' on Long Node editing
    # see https://sourceforge.net/p/freemind/bugs/1306/
    git checkout 'cc87d4a4'
    }

pkgver() {
    cd 'code' || exit
    _commits_total="$(git rev-list --count HEAD)"
    _latest_commit="$(git rev-parse --short HEAD)"
    printf '%s.r%s.%s' "${_release}" "${_commits_total}" "${_latest_commit}"
    }

build() {
    cd 'code/freemind' || exit
    env JAVA_HOME='/usr/lib/jvm/java-8-openjdk' ant
    }

package() {
    install --directory "${pkgdir}/usr/share/"
    cp --recursive 'code/bin/dist/' "${pkgdir}/usr/share/freemind/"

    install --directory "${pkgdir}/usr/bin/"
    ln --symbolic '/usr/share/freemind/freemind.sh' "${pkgdir}/usr/bin/freemind"
    chmod +x "${pkgdir}/usr/share/freemind/freemind.sh"

    local _license_dir="${pkgdir}/usr/share/licenses/freemind/"
    install --directory "${_license_dir}"
    ln --symbolic "/usr/share/freemind/license" "${_license_dir}"

    local _prefix='FreeMindWindowIconModern'
    icns2png --extract "code/freemind/images/${_prefix}.icns"
    local _dots
    for _dots in 16 32 128 256 512
        do
            install -D --mode=644 "${_prefix}_${_dots}x${_dots}x32.png" "${pkgdir}/usr/share/icons/hicolor/${_dots}x${_dots}/apps/freemind.png"
        done
    install -D --mode=644 'code/freemind/images/76812-freemind_v0.4.svg' "${pkgdir}/usr/share/icons/hicolor/scalable/apps/freemind.svg"

    install -D --mode=644 --target-directory="${pkgdir}/usr/share/applications/" 'freemind.desktop'
    }
