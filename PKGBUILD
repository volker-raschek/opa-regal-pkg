# Maintainer: Markus Pesch <markus.pesch plus apps at cryptic.systems>

pkgname=opa-regal
_pkgname=regal
pkgver=0.39.0 # renovate: datasource=github-tags depName=open-policy-agent/regal extractVersion='^v?(?<version>.*)$'
pkgrel=1
pkgdesc="Regal is a linter and language server for Rego"
arch=('armv7h' 'aarch64' 'x86_64')
url="https://github.com/open-policy-agent/${_pkgname}"
license=('Apache 2.0')
makedepends=('go')
source=(
  "$url/archive/refs/tags/v${pkgver}.zip"
)
sha512sums=('0388c7bac88012bf25de39c7a64ce87736b855f5aa6d3f2b65b8aa233a380036c17cd486da56d154e8ca9bf21e37c23c12b901cd539e8194499f72fccc8ff67e')
b2sums=('333c1a0da0475be53279f5252c133b82f291afab6a72eb75a7248006a86ecd02dd18ebd93504b61dcc657fd0692b85a0095a67daf268b4b529ffa6d7ccde6ec8')

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
