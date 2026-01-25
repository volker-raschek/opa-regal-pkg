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
sha512sums=('f166a0168818c940cea5c067eb7ae423edfff84033c475ab6ba859e9519489cca4f123546968d6ccb8d679a34abbab45dc2b925e4da13c5a57d69c94aa58d8f0')
b2sums=('15cbee40237a8f12acdfca10315b9b6e8258999b4264d70f2b069aae75288e559ecb496c591add1f53edce7cb07a74dde56830d7af2bd1185eb05545264e44ad')

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
