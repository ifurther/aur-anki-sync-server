# Maintainer: Janne Heß <jannehess@gmail.com>

pkgname=anki-sync-server
pkgver=2.0.6
pkgrel=1
pkgdesc='A personal Anki sync server (so you can sync against your own server rather than AnkiWeb)'
url='https://github.com/ankicommunity/anki-sync-server'
license=('AGPL')
conflicts=('anki-sync-server-git')
depends=('python'
         'python-orjson'
         'python-psutil' 
         'python-webob' 
         'anki') # TODO Replace anki with libanki
makedepends=('patch' 'python-setuptools')
backup=('/etc/ankisyncd/ankisyncd.conf')
source=("https://github.com/ankicommunity/${pkgname}/archive/${pkgver}.tar.gz"
        'ankiserverctl.py.patch'
        "${pkgname}.service")
sha512sums=('86d00374988d74fe2cf489982befc718ec708afa68bd91c5b52ab80339d9f4ba7ce6734846b5bc89cfef8748b8a4d24c7c62d335db28d983816e9902d9e349a4'
            'f247d0b64a8d9df9b636e1e8f9fe8894982f3a26e0cf9297cebcb8bf51b7526e451e495e15c7f1cc1c250c265d44723f710c8a13d5cabeede4ebe222d3e3dff0'
            '1d0666f17c181b4946fd37cc5d323a53d674b0b7e330eac515f4bea74e726162bd357b51e00bf162de7f2229aabec6859363f8c4aa7ce5e8e1974d72661b2860')
arch=('any')
install="${pkgname}.install"

prepare() {
	cd "${srcdir}/${pkgname}-${pkgver}"
	#patch -p0 -i "${srcdir}/ankiserverctl.py.patch"
}

package() {
	cd "${srcdir}/${pkgname}-${pkgver}"

	python setup.py install --root="${pkgdir}/" --optimize=1

	install -dm755 "${pkgdir}/etc/webapps/anki-sync-server"
	install -dm755 "${pkgdir}/var/lib/anki-sync-server"
	install -Dm644 "${srcdir}/anki-sync-server.service" "${pkgdir}/usr/lib/systemd/system/anki-sync-server.service"
	# Sanatize paths
	mv "${pkgdir}/usr/bin/ankiserverctl.py" "${pkgdir}/usr/bin/ankiserverctl"
	mv "${pkgdir}/usr/examples/example.ini" "${pkgdir}/etc/webapps/anki-sync-server/production.ini"
	mv "${pkgdir}/usr/examples/logging.conf" "${pkgdir}/etc/webapps/anki-sync-server/"
	# Remove useless files and directories
	rm -r "${pkgdir}/usr/examples" "${pkgdir}/usr/anki-bundled"
	# Fix paths
	sed -i \
		-e 's:logging.conf$:/etc/webapps/anki-sync-server/logging.conf:g' \
		-e 's:./collections:/var/lib/anki-sync-server/collections:g' \
		-e 's:./session.db:/var/lib/anki-sync-server/session.db:g' \
		-e 's:./auth.db:/var/lib/anki-sync-server/auth.db:g' \
		"${pkgdir}/etc/webapps/anki-sync-server/production.ini"
}

