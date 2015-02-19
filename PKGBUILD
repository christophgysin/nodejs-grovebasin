# Maintainer: Christoph Gysin <christoph.gysin@gmail.com>
# Contributor: Jeremy "Ichimonji10" Audet <ichimonji10 at gmail dot com>
# Contributor: Andrew Kelley <superjoe30@gmail.com>
# Contributor: superjoe <superjoe30@gmail.com>
#
# makepkg warns "Package contains reference to $pkgdir". This is OK. See:
# https://github.com/andrewrk/groovebasin/issues/214

pkgname=nodejs-groovebasin
_pkgname="${pkgname#nodejs-}"
pkgver=1.5.0
pkgrel=1
pkgdesc='Music player server with a web-based user interface inspired by Amarok 1.4'
arch=('i686' 'x86_64')
url='http://groovebasin.com/'
license=(MIT)
depends=(nodejs libgroove)
makedepends=(python2)
source=("https://github.com/andrewrk/groovebasin/archive/${pkgver}.tar.gz"
        groovebasin
        groovebasin.service)
sha256sums=('bee0ec46246c9759832f1e5a8805bc87c4451726d380f4c4e6b2def98b557305'
            '5169f64bbe305959d6c2c76f73b10c3a604586cb884c78e9b620e476f45132df'
            'f46397292c6da77464dd483c84da6440d1571cdc2ffb4ea65820ce1432ee9898')

build() {
  cd "${srcdir}/${_pkgname}-${pkgver}"
  npm \
    --python=python2 \
    run build
}

package() {
  npm \
    --python=python2 \
    install \
    --user root \
    --global \
    --prefix "${pkgdir}/usr" \
    "${srcdir}/${_pkgname}-${pkgver}"

  install -Dm755 "${srcdir}/groovebasin" "${pkgdir}/usr/bin/${_pkgname}"
  install -Dm644 "${srcdir}/${_pkgname}-${pkgver}/LICENSE" \
    "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"

  install -d -g 45 -o 45 "${pkgdir}"/var/lib/groovebasin
  install -d -g 45 -o 45 "${pkgdir}"/var/lib/groovebasin/certs

  ln -sf /usr/lib/node_modules/groovebasin/certs/self-signed-key.pem \
    "${pkgdir}"/var/lib/groovebasin/certs
  ln -sf /usr/lib/node_modules/groovebasin/certs/self-signed-cert.pem \
    "${pkgdir}"/var/lib/groovebasin/certs

  install -Dm644 "${srcdir}"/groovebasin.service \
    "${pkgdir}"/usr/lib/systemd/system/groovebasin.service
}

# vim:set ts=2 sw=2 et:
