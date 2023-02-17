# Maintainer: Alexander Bocken <alexander@bocken.org>
# Contributor: Posi <posi1981@gmail.com>
# Contributor: Johannes Löthberg <johannes@kyriasis.com>
# Contributor: Sergej Pupykin <pupykin.s+arch@gmail.com>
# Contributor: Timm Preetz <timm@preetz.us>
# Contributor: Michael 'manveru' Fellinger <m.fellinger@gmail.com>
# Contributor: Dave Pretty <david dot pretty at gmail dot com>

pkgname=anki
pkgver=2.1.58
pkgrel=1
pkgdesc="Helps you remember facts (like words/phrases in a foreign language) efficiently"
url="https://apps.ankiweb.net/"
license=('AGPL3')
arch=('x86_64')
conflicts=('anki-bin' 'anki-git' 'anki-official-binary-bundle' 'anki-qt5')
options=('!lto')
depends=(
    # anki & aqt
    'python-beautifulsoup4'
    'python-waitress'

    # anki
    'python-decorator'
    'python-markdown'
    'python-orjson'
    'python-protobuf'
    'python-pysocks'
    'python-distro'

    #aqt
    'python-flask-cors' # python-flask required for anki & aqt but a dependency of -cors
    'python-jsonschema'
    'python-requests'
    'python-send2trash'
    'python-certifi'
    'qt6-multimedia'	# recording voice
    'python-pyqt6-webengine'
    'qt6-svg'
)
makedepends=(
    'rsync'
    'ninja'
    'git'
    'cargo'
    'python-installer'
    'libxcrypt-compat'
    'nodejs'
)
optdepends=(
    'lame: record sound'
    'mpv: play sound. prefered over mplayer'
    'mplayer: play sound'
    'texlive-most: render LaTex in cards'
)
# using the tag tarballs does not work with the new (>= 2.1.55) build process.
# the '.git' folder is not included in those but is required for a sucessful build
source=("anki-${pkgver}::git+https://github.com/ankitects/anki#tag=${pkgver}"
"no-update.patch"
)
sha256sums=('SKIP'
'137827586d2a72adddaaf98599afa9fc80cdd73492d7f5cbcf4d2f6082e5f797'
)

prepare(){
    cd "anki-$pkgver"
    # pro-actively prevent "module not found" error (TODO: verify whether still required)
    [ -d ts/node_modules ] && rm -r ts/node_modules
    patch -p1 < "$srcdir/no-update.patch"
}

build() {
    cd "anki-$pkgver"
    ./tools/build
}

package() {
    cd "anki-$pkgver"
    for file in out/wheels/*.whl; do
    	python -m installer --destdir="$pkgdir" $file
    done

    install -Dm644 qt/bundle/lin/anki.desktop "$pkgdir"/usr/share/applications/anki.desktop
    install -Dm644 qt/bundle/lin/anki.png "$pkgdir"/usr/share/pixmaps/anki.png
    # TODO: verify whether still required
    find $pkgdir -iname __pycache__ | xargs -r rm -rf
    find $pkgdir -iname direct_url.json | xargs -r rm -rf
}
