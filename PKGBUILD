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
_os="$(
  uname \
    -o)"
_evmfs_available="$(
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
_locale="$(
  locale |
    grep \
      "LANG=" |
      awk \
        -F \
          "=" \
        '{print $1}')"
if [[ ! -v "_en" ]]; then
  _en="true"
  if [[ "${_locale}" == "C.UTF-8" ]]; then
    _en="true"
  fi
fi
if [[ ! -v "_it" ]]; then
  _it="true"
  if [[ "${_locale}" == "it_IT.UTF-8" ]]; then
    _it="true"
  fi
fi
if [[ ! -v "_like" ]]; then
  if [[ "${_it}" == "true" ]]; then
    _like="mo-me-lo-segno"
  fi
  if [[ "${_en}" == "true" ]]; then
    _like="never-gonna-give-you-up"
  fi
fi
if [[ ! -v "_boost_pkgver" ]]; then
  _boost_pkgver="$(
    pacman \
      -Qi \
        "boost-libs" |
      grep \
        "Version" |
        awk \
          '{print $3}' |
          rev |
            cut \
              -d \
                "-" \
              -f \
                "1" \
              --complement |
              rev || \
    echo \
      "null")"
fi
if [[ ! -v "_git" ]]; then
  _git="false"
fi
if [[ ! -v "_git_service" ]]; then
  _git_service="github"
fi
if [[ "${_os}" == "Android" ]]; then
  _compiler="clang"
elif [[ "${_os}" == "GNU/Linux" ]]; then
  _compiler="gcc"
fi
if [[ ! -v "_cmake_generator" ]]; then
  _cmake_generator="make"
  # _cmake_generator="ninja"
fi
if [[ ! -v "_archive_format" ]]; then
  if [[ "${_git}" == "true" ]]; then
    if [[ "${_evmfs}" == "true" ]]; then
      _archive_format="bundle"
    elif [[ "${_evmfs}" == "false" ]]; then
      _archive_format="git"
    fi
  elif [[ "${_git}" == "false" ]]; then
    _archive_format="tar.gz"
  fi
fi
_pkg=solidity
pkgbase="${_pkg}"
pkgname+=(
  "${_pkg}"
)
pkgver="0.8.30"
_commit="73712a01b2de56d9ad91e3b6936f85c90cb7de36"
_bundle_commit="142aa62e6805505b6a06cbeeec530f5c8bf0bfdd"
_0_8_30_1_commit="8b8767a80b768e2ca75386f4ce224c15f77dc286"
pkgrel=30
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
_http="https://${_git_service}.com"
# In late 2025 or 2026, according
# to Github, Solidity publishing
# namespace seems to have been moved
# moved from 'ethereum' to 'argotorg'
# _ns="ethereum"
# Despite this, Solidity 0.8.30 requires
# changes to be built with Boost versions
# later than 1.83 which are only published
# on The Martian Company namespaces.
_boost_oldest="$(
  printf \
    "%s\n%s" \
    "1.89" \
    "${_boost_pkgver}" |
    sort \
      -V |
      head \
        -n \
	  1)"
if [[ "${_boost_oldest}" == "1.89" ]]; then
  _ns="themartiancompany"
  if [[ "${_evmfs}" == "true" ]]; then
  _commit="${_bundle_commit}"
  elif [[ "${_evmfs}" == "false" ]]; then
    _commit="${_0_8_30_1_commit}"
  fi
elif [[ "${_boost_oldest}" != "1.89" ]]; then
  _ns="argotorg"
fi
url="${_http}/${_ns}/${_pkg}"
license=(
  "GPL-3.0-or-later"
)
depends=(
  "boost-libs"
  "fmt"
  "nlohmann-json"
  "range-v3"
)
optdepends=(
  "cvc4: SMT checker"
  "z3: SMT checker"
)
makedepends=(
  "boost"
  "cmake3"
  "${_compiler}"
  "${_cmake_generator}"
  "fmt"
  "nlohmann-json"
  "range-v3"
)
if [[ "${_os}" == "Android" ]]; then
  makedepends+=(
    "boost-headers"
    "boost-static"
  )
fi
if [[ "${_git}" == "true" ]]; then
  makedepends+=(
    "git"
  )
fi
if [[ "${_evmfs}" == "true" ]]; then
  makedepends+=(
    "evmfs"
  )
fi
checkdepends=(
  "evmone"
)
group=(
  "ethereum"
  "hip"
)
provides=(
  "solc=${pkgver}"
  "${_pkg}-bin=${pkgver}"
  "${_pkg}-git=${pkgver}"
)
conflicts=(
  "solc"
  "${_pkg}-bin"
  "${_pkg}-git"
)
if [[ "${_git}" == "false" ]]; then
  if [[ "${_boost_oldest}" == "1.89" ]]; then
    _tag="${_commit}"
    _tag_name="commit"
    _tarname="${_pkg}-${_tag}"
  elif [[ "${_boost_oldest}" != "1.89" ]]; then
    _tag="${pkgver}"
    _tag_name="pkgver"
    _tarname="${_pkg}_${_tag}"
  fi
elif [[ "${_git}" == "true" ]]; then
  _tag="${_commit}"
  _tag_name="commit"
  _tarname="${_pkg}-${_tag}"
fi
_0_8_30_1_tarname="${_pkg}-${_0_8_30_1_commit}"
_tarfile="${_tarname}.${_archive_format}"
_0_8_30_1_tarfile="${_0_8_30_1_tarname}.${_archive_format}"
_bundle_sum="77860b58f9d6c4a9a9cb1ceaae7ebe5d856f91f3ccd96f67d5ea6a019d79d1fb"
_bundle_sig_sum="7f737e7a88fdb8e96b428974592def4bbdf5bf24656b12ac5af76084b7fca095"
_0_8_30_1_sum="ee1e8be598e10ab639454e3cfe7cde53b6573afc297ce72a9cae12c569cd0051"
_0_8_30_1_sig_sum="7c36822399acda42434958d07ab6fcfbff45a2f654bc06edeed046c362e9e186"
_github_release_sum="5e8d58dff551a18205e325c22f1a3b194058efbdc128853afd75d31b0568216d"
_github_release_sha512_sum="b08733619a4c1398a2b80d0fec83d56b3769af8dfa01a028c71ff89985f5c93d12c3c7d8bbcec29bb0816a9cc1d56bb099010e59a203bcf917b87ff1b0cf0241"
# Dvorak
_evmfs_ns="0x87003Bd6C074C713783df04f36517451fF34CBEf"
_kid_ns="0x926acb6aA4790ff678848A9F1C59E578B148C786"
_evmfs_network="100"
_evmfs_address="0x69470b18f8b8b5f92b48f6199dcb147b4be96571"
_evmfs_dir="evmfs://${_evmfs_network}/${_evmfs_address}/${_evmfs_ns}"
_evmfs_uri="${_evmfs_dir}/${_bundle_sum}"
_evmfs_src="${_tarfile}::${_evmfs_uri}"
_sig_uri="${_evmfs_dir}/${_bundle_sig_sum}"
_sig_src="${_tarfile}.sig::${_sig_uri}"
_0_8_30_1_uri="${_evmfs_dir}/${_0_8_30_1_sum}"
_0_8_30_1_src="${_0_8_30_1_tarfile}::${_0_8_30_1_uri}"
_0_8_30_1_sig_uri="${_evmfs_dir}/${_0_8_30_1_sig_sum}"
_0_8_30_1_sig_src="${_0_8_30_1_tarfile}.sig::${_0_8_30_1_sig_uri}"
if [[ "${_evmfs}" == "true" ]]; then
  if [[ "${_git}" == "false" ]]; then
    makedepends+=(
      "ur"
      "${_like}"
    )
    _src=""
  elif [[ "${_git}" == "false" ]]; then
    _src="${_evmfs_src}"
    _sum="${_bundle_sum}"
    source+=(
      "${_sig_src}"
    )
    sha256sums+=(
      "${_bundle_sig_sum}"
    )
    if [[ "${_boost_oldest}" == "1.89" ]]; then
      source+=(
        "${_0_8_30_1_src}"
        "${_0_8_30_1_sig_src}"
      )
      sha256sums+=(
        "${_0_8_30_1_sum}"
        "${_0_8_30_1_sig_sum}"
      )
    fi
  fi
elif [[ "${_evmfs}" == "false" ]]; then
  if [[ "${_git}" == true ]]; then
    _src="${_tarname}::git+${url}#${_tag_name}=${_tag}?signed"
    _sum="SKIP"
  elif [[ "${_git}" == false ]]; then
    if [[ "${_git_service}" == "github" ]]; then
      if [[ "${_tag_name}" == "commit" ]]; then
        _uri="${url}/archive/${_commit}.${_archive_format}"
        _sum="SKIP"
      elif [[ "${_tag_name}" == "pkgver" ]]; then
        _uri="${url}/releases/download/v${pkgver}/${_pkg}_${pkgver}.tar.gz"
	_sum="${_github_release_sum}"
        sha512sums=(
          "${_github_release_sha512_sum}"
        )
      fi
    elif [[ "${_git_service}" == "gitlab" ]]; then
      if [[ "${_tag_name}" == "commit" ]]; then
        _uri="${_url}/-/archive/${_tag}/${_tag}.${_archive_format}"
      fi
    fi
    _src="${_tarfile}::${_uri}"
  fi
fi
if [[ "${_src}" != "" ]]; then
  source+=(
    "${_src}"
  )
  sha256sums+=(
    "${_sum}"
  )
fi
validpgpkeys=(
  # Truocolo
  #   <truocolo@aol.com>
  '97E989E6CF1D2C7F7A41FF9F95684DBE23D6A3E9'
  #   <truocolo@0x6E5163fC4BFc1511Dbe06bB605cc14a3e462332b>
  'F690CBC17BD1F53557290AF51FC17D540D0ADEED'
  # Pellegrino Prevete (dvorak)
  #   <dvorak@0x87003Bd6C074C713783df04f36517451fF34CBEf>
  '12D8E3D7888F741E89F86EE0FEC8567A644F1D16'
)

_git_unbundle() {
  local \
    _tarname="${1}" \
    _bundle \
    _repo \
    _msg=()
  _bundle="${srcdir}/${_tarname}.bundle"
  _repo="${srcdir}/${_tarname}"
  _msg=(
    "Cloning '${_bundle}' into '${_repo}'."
  )
  msg \
    "${_msg[*]}"
  git \
    clone \
      "${_bundle}" \
      "${_repo}" || \
  git \
    -C \
      "${_repo}" \
      pull || \
  true
}

_git_unbundle_update() {
  local \
    _repo="${1}" \
    _bundle="${2}" \
    _repo \
    _msg=()
  _bundle_name="$(
    basename \
      "${_bundle}")"
  _msg=(
    "Updating '${_repo}' from '${_bundle}'."
  )
  msg \
    "${_msg[*]}"
  git \
    -C \
      "${_repo}" \
      remote \
        add \
          "${_bundle_name}" \
	  "${_bundle}" ||
  true
  git \
    -C \
      "${_repo}" \
    pull \
      "${_bundle_name}" || \
  true
}

prepare() {
  if [[ "${_evmfs}" == "true" ]]; then
    if [[ "${_git}" == "false" ]]; then
      ur \
        "${_like}"
    elif [[ "${_git}" == "true" ]]; then
      _git_unbundle \
        "${_tarname}"
      if [[ "${_boost_oldest}" == "1.89" ]]; then
        _git_unbundle_update \
          "${srcdir}/${_tarname}" \
          "${srcdir}/${_pkg}-${_0_8_30_1_commit}"
      fi
    fi
  fi
}

_bin_get() {
  local \
    _cc \
    _bin
  _cc="$(
    command \
      -v \
      "cc" \
      "g++" \
      "clang++" | \
      head \
        -n \
          1)"
  _bin="$(
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
  _cc="$(
    command \
      -v \
      "${_compiler}")"
  if [[ "${_compiler}" == "gcc" ]]; then
    _cxx_compiler="g++"
  elif [[ "${_compiler}" == "clang" ]]; then
   _cxx_compiler="${_compiler}++"
  fi
  _cxx="$(
    command \
      -v \
      "${_cxx_compiler}")"
  _cmake_opts=(
    --trace-expand 
    # -G
    #   "Ninja"
    -D
      CMAKE_BUILD_TYPE="Release"
    -D
      CMAKE_INSTALL_PREFIX="/usr/"
    -D
      ONLY_BUILD_SOLIDITY_LIBRARIES="OFF"
    -D
      PEDANTIC="ON"
    -D
      PROFILE_OPTIMIZER_STEPS="OFF"
    -D
      SOLC_LINK_STATIC="OFF"
    -D
      SOLC_STATIC_STDLIBS="OFF"
    -D
      STRICT_NLOHMANN_JSON_VERSION="OFF"
    -D
      STRICT_Z3_VERSION="OFF"
    -D
      TESTS="${_tests}"
    -D
      USE_LD_GOLD="OFF"
    # Introduced somewhere after 0.8.28
    -D
      IGNORE_VENDORED_DEPENDENCIES="ON"
    # Disabled somewhere after 0.8.28
    # -D
    #   USE_SYSTEM_LIBRARIES="OFF"
    -S
      "${srcdir}/${_pkg}_${pkgver}/"
    -Wno-dev
  )
  export \
    CC="${_cc}" \
    CXX="${_cxx}"
  CC="${_cc}" \
  CXX="${_cxx}" \
  cmake \
    -B \
      "${srcdir}/${_pkg}_${pkgver}/build/" \
    "${_cmake_opts[@]}"
  CC="${_cc}" \
  CXX="${_cxx}" \
  cmake \
    --build \
      "${srcdir}/${_pkg}_${pkgver}/build/" \
    2>&1 > \
    "${srcdir}/build.log"
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
  ls
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
  "${srcdir}/${_pkg}_${pkgver}/build/test/soltest" \
    -p \
      true -- \
    --testpath \
      "${srcdir}/${_pkg}_${pkgver}/test/"
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
      "${pkgdir}/usr/share/doc/${_pkg}/"
  # Install the software.
  DESTDIR="${pkgdir}/" \
  cmake \
    --install \
      "${srcdir}/${_pkg}_${pkgver}/build/"
  # Set version link
  ln \
    -s \
    "$(_bin_get)/solc" \
    "${pkgdir}/usr/bin/solc${pkgver}"
  # Install the documentation.
  install \
    -Dm644 \
    "${srcdir}/${_pkg}_${pkgver}/README.md" \
    "${pkgdir}/usr/share/doc/${_pkg}/"
  cp \
    -r \
    "${srcdir}/${_pkg}_${pkgver}/docs/"* \
    "${pkgdir}/usr/share/doc/${_pkg}/"
  find \
    "${pkgdir}/usr/share/doc/${_pkg}/" \
    -type \
      d \
    -exec \
      chmod \
        755 \
	{} +
  find \
    "${pkgdir}/usr/share/doc/${_pkg}/" \
    -type \
      f \
    -exec \
      chmod \
      644 \
      {} +
}
