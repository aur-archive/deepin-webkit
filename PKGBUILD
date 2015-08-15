# Maintainer: Xu Fasheng <fasheng.xu[AT]gmail.com>
# Contributor: Josip Ponjavic <josipponjavic at gmail dot com>

pkgname=deepin-webkit
pkgver=git20131220153235
pkgrel=1
pkgdesc='A fork of webkit-gtk for Linux Deepin desktop eviroment'
arch=('i686' 'x86_64')
url="http://www.linuxdeepin.com/"
license=('LGPL + GPL + BSD + other')
depends=('libxt' 'libxslt' 'sqlite' 'libsoup' 'enchant' 'libgl' 'geoclue' 'gtk3' 'gstreamer0.10-base-plugins' 'libsecret' 'libwebp' 'harfbuzz-icu')
makedepends=('gtk2' 'gperf' 'gobject-introspection' 'python2' 'mesa' 'gtk-doc' 'gawk' 'chrpath' 'bison' 'perl' 'flex' 'which' 'gettext')
optdepends=('gtk2: Netscape plugin support')
provides=("${pkgname%-*}")
options=(!libtool !emptydirs)

_fileurl=http://packages.linuxdeepin.com/deepin/pool/main/d/deepin-webkit/deepin-webkit_1.8.2.1+git20131220153235~9ac8cf628.tar.gz
source=("${_fileurl}"
        "webkit-gtk-1.8.2-bison-2.6.patch"
	    "DerivedSources.tar.gz")
md5sums=('75e2b2c18db9a7ae1f49ec02243e37ba'
         '4d6a5dc625d442e7883e0f27943a9db9'
         'ac866fe0094877f5bd0f22a0eff5bd4d')

_filename="$(basename "${_fileurl}")"
_filename="${_filename%.tar.gz}"
_innerdir="${_filename/_/-}"

_install_copyright_and_changelog() {
    mkdir -p "${pkgdir}/usr/share/doc/${pkgname}"
    cp -f debian/copyright "${pkgdir}/usr/share/doc/${pkgname}/"
    gzip -c debian/changelog > "${pkgdir}/usr/share/doc/${pkgname}/changelog.gz"
}

build() {
    cd "${srcdir}/${_innerdir}"

    export CFLAGS="$CFLAGS -Wall -Wl,--as-needed"
    export LDFLAGS="$LDFLAGS -Wl,--no-keep-memory"

    patch -p1 -i "${srcdir}"/webkit-gtk-1.8.2-bison-2.6.patch

    PYTHON=/usr/bin/python2 ./configure --prefix=/usr \
        --disable-silent-rules \
        --with-gtk=3.0 \
        --disable-webgl
    make
}

package() {
    cd "${srcdir}/${_innerdir}"

    make DESTDIR="$pkgdir" install
    rm -rf "$pkgdir"/usr/{bin,include,share}
    mv "${pkgdir}"/usr/lib/pkgconfig/webkitgtk-3.0.pc "${pkgdir}"/usr/lib/pkgconfig/deepin-webkit-3.0.pc
    _install_copyright_and_changelog
}
