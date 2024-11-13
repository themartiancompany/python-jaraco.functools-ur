# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Pellegrino Prevete (tallero) <pellegrinoprevete@gmail.com>
# Maintainer: Chih-Hsuan Yen <yan12125@archlinux.org>
# Contributor: Kyle Keen <keenerd@gmail.com>

_git="false"
_py="python"
_pyver="$( \
  "${_py}" \
    -V | \
    awk \
      '{print $2}')"
_pymajver="${_pyver%.*}"
_pyminver="${_pymajver#*.}"
_pynextver="${_pymajver%.*}.$(( \
  ${_pyminver} + 1))"
_pkg=requests
pkgname="${_py}-${_pkg}"
_proj="jaraco"
_pkg="${_proj}.functools"
_Pkg="${_proj}_functools"
pkgname="${_py}-${_pkg}"
# https://github.com/jaraco/jaraco.functools/blob/main/NEWS.rst
pkgver=4.1.0
# curl https://api.github.com/repos/jaraco/jaraco.functools/git/ref/tags/v$pkgver | jq -r .object.sha
_tag="a51deece0e54c37c5811565a2997bce2a77fb0a7"
pkgrel=1
pkgdesc='Functools like those found in stdlib'
arch=(
  'any'
)
_http="https://github.com"
_ns="${_proj}"
url="${_http}/${_ns}/${_pkg}"
license=(
  'MIT'
)
depends=(
  "${_py}>=${_pymajver}"
  "${_py}<${_pynextver}"
  "${_py}-more-itertools"
)
makedepends=(
  "${_py}-build"
  "${_py}-installer"
  "${_py}-setuptools-scm"
  "${_py}-wheel"
)
checkdepends=(
  "${_py}-pytest"
  "${_py}-jaraco.classes"
)
conflicts=(
  "${_py}-jaraco"
)
replaces=(
  "${_py}-jaraco"
)
_pypi="https://pypi.io/packages/source"
_tarname="${_Pkg}-${pkgver}"
if [[ "${_git}" == "true" ]]; then
  makedepends+=(
    'git'
  )
  _src="${_tarname}::git+${url}?signed#tag=$_tag"
  _sum="SKIP"
elif [[ "${_git}" == "false" ]]; then
  _src="${_tarname}.tar.gz::${_pypi}/${_pkg::1}/${_pkg}/${_Pkg}-${pkgver}.tar.gz"
  # _src="${_pkg}-${pkgver}.tar.gz::${url}/archive/refs/tags/v${pkgver}.tar.gz"
  _sum="70f7e0e2ae076498e212562325e805204fc092d7b4c17e0e86c959e249701a9d"
fi

source=(
  "${_src}"
)
sha256sums=(
  "${_sum}"
)
validpgpkeys=(
  # https://github.com/jaraco.gpg
  'CE380CF3044959B8F377DA03708E6CB181B4C47E'
)

pkgver() {
  local \
    _pkgver
  _pkgver="${pkgver}"
  if [[ "${_git}" == "true" ]]; then
    cd \
      "${_tarname}"
    _pkgver="$( \
      git \
        describe \
        --tags | \
        sed \
	's/^v//')"
  fi
  echo \
    "${_pkgver}"
}

build() {
  ls
  cd \
    "${_tarname}"
  "${_py}" \
    -m \
      build \
    --wheel \
    --no-isolation
}

check() {
  cd \
    "${_tarname}"
  pytest
}

package() {
  cd \
    "${_tarname}"
  "${_py}" \
    -m \
      installer \
    --destdir="${pkgdir}" \
    dist/*.whl
  install \
    -Dm644 \
    LICENSE \
    "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}

