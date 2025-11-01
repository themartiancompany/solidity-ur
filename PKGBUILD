# SPDX-License-Identifier: AGPL-3.0

#    ----------------------------------------------------------------------
#    Copyright © 2024, 2025  Pellegrino Prevete
#
#    All rights reserved
#    ----------------------------------------------------------------------
#
#    This program is free software: you can redistribute it and/or modify
#    it under the terms of the GNU Affero General Public License as published by
#    the Free Software Foundation, either version 3 of the License, or
#    (at your option) any later version.
#
#    This program is distributed in the hope that it will be useful,
#    but WITHOUT ANY WARRANTY; without even the implied warranty of
#    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#    GNU Affero General Public License for more details.
#
#    You should have received a copy of the GNU Affero General Public License
#    along with this program.  If not, see <https://www.gnu.org/licenses/>.

# Maintainers:
#   Truocolo
#     <truocolo@aol.com>
#     <truocolo@0x6E5163fC4BFc1511Dbe06bB605cc14a3e462332b>
#   Pellegrino Prevete (dvorak)
#     <pellegrinoprevete@gmail.com>
#     <dvorak@0x87003Bd6C074C713783df04f36517451fF34CBEf>
# Contributors:
#   Matheus
#     <matheusgwdl@protonmail.com>
#   Serge K
#     <arch@phnx47.net>
#   Nicola Squartini
#     <tensor5@gmail.com>

# shellcheck disable=SC2034
# shellcheck disable=SC2154
_os="$( \
  uname \
    -o)"
_evmfs_available="$( \
  command \
    -v \
    "evmfs" || \
    true)"
if [[ ! -v "_evmfs" ]]; then
  if [[ "${_evmfs_available}" != "" ]]; then
    _evmfs="true"
  elif [[ "${_evmfs_available}" == "" ]]; then
    _evmfs="false"
  fi
fi
if [[ ! -v "_git" ]]; then
  _git="false"
fi
if [[ "${_os}" == "Android" ]]; then
  _compiler="clang"
elif [[ "${_os}" == "GNU/Linux" ]]; then
  _compiler="gcc"
fi
_pkg=solidity
pkgbase="${_pkg}"
pkgname+=(
  "${_pkg}"
)
pkgver="0.8.30"
_commit="73712a01b2de56d9ad91e3b6936f85c90cb7de36"
_bundle_commit="142aa62e6805505b6a06cbeeec530f5c8bf0bfdd"
pkgrel="1"
pkgdesc="Smart contract programming language."
arch=(
  "x86_64"
  "i686"
  "aarch64"
  "arm"
  "armv7l"
  "armv6l"
  "mips"
  "powerpc"
  "pentium4"
)
_http="https://github.com"
_ns="ethereum"
url="${_http}/${_ns}/${pkgname}"
license=(
  "GPL-3.0-or-later"
)
depends=(
  "boost-libs"
)
optdepends=(
  "cvc4: SMT checker"
  "z3: SMT checker"
)
makedepends=(
  "boost"
  "cmake"
  "${_compiler}"
)
if [[ "${_os}" == "Android" ]]; then
  makedepends+=(
    "boost-headers"
    "boost-static"
  )
fi
checkdepends=(
  "evmone"
)
provides=(
  "solc=${pkgver}"
  "solidity-bin=${pkgver}"
  "solidity-git=${pkgver}"
)
conflicts=(
  "solc"
  "solidity-bin"
  "solidity-git"
)
_sum="77860b58f9d6c4a9a9cb1ceaae7ebe5d856f91f3ccd96f67d5ea6a019d79d1fb"
_sig_sum="7f737e7a88fdb8e96b428974592def4bbdf5bf24656b12ac5af76084b7fca095"
_sha512_sum="b08733619a4c1398a2b80d0fec83d56b3769af8dfa01a028c71ff89985f5c93d12c3c7d8bbcec29bb0816a9cc1d56bb099010e59a203bcf917b87ff1b0cf0241"
if [[ "${_evmfs}" == "true" ]]; then
  _uri=""
elif [[ "${_evmfs}" == "false" ]]; then
  if [[ "${_git}" == "false" ]]; then
    _uri="${url}/releases/download/v${pkgver}/${pkgname}_${pkgver}.tar.gz"
  fi
fi
_src="${_pkg}-v${pkgver}.tar.gz::${_uri}"
source=(
  "${_src}"
)
sha512sums=(
  "${_sha512_sum}"
)

_bin_get() {
  local \
    _cc \
    _bin
  _cc="$( \
    command \
      -v \
      "cc" \
      "g++" \
      "clang++" | \
      head \
        -n \
          1)"
  _bin="$( \
    dirname \
      "${_cc}")"
  echo \
    "${_bin}"
}

_compile() {
  local \
    _tests="${1}" \
    _cmake_opts=() \
    _cc \
    _cxx \
    _cxx_compiler
  _cc="$( \
    command \
      -v \
      "${_compiler}")"
  if [[ "${_compiler}" == "gcc" ]]; then
    _cxx_compiler="g++"
  elif [[ "${_compiler}" == "clang" ]]; then
   _cxx_compiler="${_compiler}++"
  fi
  _cxx="$( \
    command \
      -v \
      "${_cxx_compiler}")"
  _cmake_opts=(
    -D CMAKE_BUILD_TYPE="None"
    -D CMAKE_INSTALL_PREFIX="/usr/"
    -D ONLY_BUILD_SOLIDITY_LIBRARIES="OFF"
    -D PEDANTIC="ON"
    -D PROFILE_OPTIMIZER_STEPS="OFF"
    -D SOLC_LINK_STATIC="OFF"
    -D SOLC_STATIC_STDLIBS="OFF"
    -D STRICT_NLOHMANN_JSON_VERSION="OFF"
    -D STRICT_Z3_VERSION="OFF"
    -D TESTS="${_tests}"
    -D USE_LD_GOLD="OFF"
    -D USE_SYSTEM_LIBRARIES="OFF"
    -S "${srcdir}/${pkgname}_${pkgver}/"
    -Wno-dev
  )
  export \
    CC="${_cc}" \
    CXX="${_cxx}"
  CC="${_cc}" \
  CXX="${_cxx}" \
  cmake \
    -B \
      "${srcdir}/${pkgname}_${pkgver}/build/" \
    "${_cmake_opts[@]}"
  CC="${_cc}" \
  CXX="${_cxx}" \
  cmake \
    --build \
      "${srcdir}/${pkgname}_${pkgver}/build/"
}

build()
{
  local \
    _tests_switch=() \
    _tests_switch_status
  _tests_switch=(
    "OFF"
    # "ON"
  )
  for _tests_switch_status \
    in "${_tests_switch[@]}"; do
    _compile \
      "${_tests_switch_status}"
  done
}

check()
{
  _compile \
    "ON"
  "${srcdir}/${pkgname}_${pkgver}/build/test/soltest" \
    -p \
      true -- \
    --testpath \
      "${srcdir}/${pkgname}_${pkgver}/test/"
  _compile \
    "OFF"
}

package_solidity() {
  conflicts=(
    "solc"
    "solidity-bin"
    "solidity-git"
  )
  # Assure that the directories exist.
  mkdir \
    -p \
      "${pkgdir}/usr/share/doc/${pkgname}/"
  # Install the software.
  DESTDIR="${pkgdir}/" \
  cmake \
    --install \
      "${srcdir}/${pkgname}_${pkgver}/build/"
  # Set version link
  ln \
    -s \
    "$(_bin_get)/solc" \
    "${pkgdir}/usr/bin/solc${pkgver}"
  # Install the documentation.
  install \
    -Dm644 \
    "${srcdir}/${pkgname}_${pkgver}/README.md" \
    "${pkgdir}/usr/share/doc/${pkgname}/"
  cp \
    -r \
    "${srcdir}/${pkgname}_${pkgver}/docs/"* \
    "${pkgdir}/usr/share/doc/${pkgname}/"
  find \
    "${pkgdir}/usr/share/doc/${pkgname}/" \
    -type \
      d \
    -exec \
      chmod \
        755 \
	{} +
  find \
    "${pkgdir}/usr/share/doc/${pkgname}/" \
    -type \
      f \
    -exec \
      chmod \
      644 \
      {} +
}
