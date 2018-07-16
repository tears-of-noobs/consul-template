# Maintainer: Christian Rebischke <chris.rebischke@archlinux.org>
# Contributor: Tyler Langlois <ty |at| tjll |dot| net>
# Contributor: Tim Meusel <tim@bastelfreak.de>
pkgname=consul-template
pkgver=0.19.4
pkgrel=3
pkgdesc='Template rendering, notifier, and supervisor for HashiCorp Consul and Vault data'
arch=('x86_64')
url='https://github.com/hashicorp/consul-template'
license=('MPL')
backup=("etc/${pkgname}/${pkgname}.hcl")
makedepends=('go-pie' 'git')
depends=('glibc')
optdepends=('consul: interpolate values from a distributed key/value store'
            'vault: reference secure secrets in template files')
_consul_template_commit='111a80441153783544817b7c6a9c08633308b9a8'
source=("git+https://github.com/hashicorp/consul-template#commit=${_consul_template_commit}"
        "${pkgname}.service"
        "${pkgname}.hcl")
sha512sums=('SKIP'
            '8b187ff470fb10b47b815b2faaad836ac369071c8ce7e353ec0cbc98e3b1ac2ffc9c892244ac492be1285caa303c4b5fd0a22df3bdb2a037fca1b06c7b24084b'
            'b2acfbb4bf389b1d95ca9a5f2dfe9be85444c20efdae63f0e6e34d2f33a16ca1d089e6510b6867f74c3b4390a097952ab235c55e4023245e61cc4318622d5674')

prepare() {
  export GOPATH="${srcdir}"
  export PATH="$PATH:$GOPATH/bin"
  ls -la
  mkdir -p src/github.com/hashicorp/
  mv "${pkgname}" src/github.com/hashicorp/
  ls -la
  mv ../feature-service-by-tag.patch src/github.com/hashicorp/

  ls -la
  patch -Np0 -i src/github.com/hashicorp/feature-service-by-tag.patch
}

build() {
  cd src/github.com/hashicorp/"${pkgname}"
  go build -o consul-template-binary
}

package() {
  cd src/github.com/hashicorp/"${pkgname}"
  install -Dm755 consul-template-binary "${pkgdir}/usr/bin/consul-template"
  install -Dm644 "${srcdir}/${pkgname}.hcl" "${pkgdir}/etc/${pkgname}/${pkgname}.hcl"
  install -Dm644 "${srcdir}/${pkgname}.service" "${pkgdir}/usr/lib/systemd/system/${pkgname}.service"
}
