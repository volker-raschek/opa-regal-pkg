# Maintainer: Markus Pesch <markus.pesch plus apps at cryptic.systems>

pkgname=opa-regal
_pkgname=regal
pkgver=0.38.1 # renovate: datasource=github-tags depName=open-policy-agent/regal extractVersion='^v?(?<version>.*)$'
pkgrel=1
pkgdesc="Regal is a linter and language server for Rego"
arch=('armv7h' 'aarch64' 'x86_64')
url="https://github.com/open-policy-agent/${_pkgname}"
license=('Apache 2.0')
makedepends=('go')
source=(
  "$url/archive/refs/tags/v${pkgver}.zip"
)
sha512sums=('5d6f093b17743d22d0fa65e850f8e55f5b55150d48d66b185af46858977ad7df51f6071ed987a3d4bcbf5e068e6e2735f5cf256f19b61b2fd3fac2a9822e117e')
b2sums=('2a6f8bef4ec0f77aad7544d83d8b759ab45278fd66edee207073870c08a53b599bbcbbef4c6a2ecbaeaa8d82a015b4822417d2f2c02501086bc1209ec78b9fca')

prepare() {
  cd ${_pkgname}-${pkgver}

  export GONOSUMDB="${GONOSUMDB}"
  export GOPATH="${srcdir}"
  export GOPROXY="${GOPROXY}"

  env | sort | grep -E '^C?GO'

  go mod download -modcacherw
}

build() {
  cd "${_pkgname}-${pkgver}"

  # https://wiki.archlinux.org/title/Go_package_guidelines
  export CGO_CPPFLAGS="${CPPFLAGS}"
  export CGO_CFLAGS="${CFLAGS}"
  export CGO_CXXFLAGS="${CXXFLAGS}"
  export CGO_LDFLAGS="${LDFLAGS}"

  export GONOSUMDB="${GONOSUMDB}"
  export GOPATH="${srcdir}"
  export GOPROXY="${GOPROXY}"

  env | sort | grep -E '^C?GO'

  go build -v \
    -buildmode=pie \
    -mod=readonly \
    -modcacherw \
    -ldflags "\
      -s -w
      -X github.com/open-policy-agent/regal/pkg/version.Version=${pkgver}
      -X github.com/open-policy-agent/regal/pkg/version.Commit=${pkgver}
      -X github.com/open-policy-agent/regal/pkg/version.Timestamp=$(date --iso-8601=seconds)
      -X github.com/open-policy-agent/regal/pkg/version.Hostname=AUR
    " \
    -trimpath \
    -o ${_pkgname} .

  ${_pkgname} completion bash > ${_pkgname}.bash
  ${_pkgname} completion fish > ${_pkgname}.fish
  ${_pkgname} completion zsh > ${_pkgname}.zsh
}

package() {
  # binary
  install -D --mode 0755 "${_pkgname}-${pkgver}/${_pkgname}" "$pkgdir/usr/bin/${_pkgname}"

  # completions
  install -D --mode 0644 "${_pkgname}-${pkgver}/${_pkgname}.bash" "$pkgdir/usr/share/bash-completion/completions/${_pkgname}"
  install -D --mode 0644 "${_pkgname}-${pkgver}/${_pkgname}.fish" "$pkgdir/usr/share/fish/vendor_completions.d/${_pkgname}.fish"
  install -D --mode 0644 "${_pkgname}-${pkgver}/${_pkgname}.zsh" "$pkgdir/usr/share/zsh/site-functions/_${_pkgname}"

  # license
  install -D --mode 0755 "${_pkgname}-${pkgver}/LICENSE" "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}
