# Maintainer:

_pkgname="python-deadlib"
pkgbase="$_pkgname"
pkgdesc="Python dead batteries. See PEP 594"
pkgver=3.13.0
pkgrel=1
url="https://github.com/youknowone/python-deadlib"
license=('PSF-2.0')
arch=('any')

special_deps=(
  ["aifc"]="('python-chunk' 'python-audioop')"
)
depends=(
  'python'
)
makedepends=(
  'python-build'
  'python-installer'
  'python-setuptools'
  'python-wheel'
)

_pkgsrc="$_pkgname-$pkgver"
_pkgext="tar.gz"
source=(
  "$_pkgsrc.$_pkgext::$url/archive/refs/tags/v$pkgver.$_pkgext"
)
sha256sums=(
  'bde43692d5a1de2a33777ff99f4c4344f7cca3a49362b2484a13870706194613'
)

_modules=(
  aifc
  asynchat
  asyncore
  cgi
  cgitb
  chunk
  crypt
  imghdr
  mailcap
  nntplib
  pipes
  smtpd
  sndhdr
  sunau
  telnetlib
  uu
  xdrlib
)

build() {
  for i in "${_modules[@]}"; do
    (
      cd "$_pkgsrc/$i"
      python -m build --wheel --no-isolation --skip-dependency-check
    )
  done
}

for i in "${_modules[@]}"; do
  _modulename=python-standard-$i
  pkgname+=(
    "$_modulename"
  )
  # create package_* function
  # Note: variables inside the package function must be escaped
  deps=${special_deps["$i"]}
  eval "$(cat <<_packaging_functions
    package_${_modulename}() {
      pkgdesc="Standard library $i redistribution."
      depends+=${special_deps[$i]}
      echo "${depends[@]}"
      provides+=(
        "python-$i"
      )
      conflicts+=(
        "python-$i"
      )
      cd "$_pkgsrc/$i"
      python -m installer --destdir="\$pkgdir/" dist/*.whl
      install -Dm644 LICENSE "\${pkgdir}/usr/share/licenses/${_modulename}/LICENSE"
    }
_packaging_functions
  )"
done
