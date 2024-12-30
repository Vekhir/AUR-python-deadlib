# Maintainer:

_pkgname="python-deadlib"
pkgbase="$_pkgname"
pkgname=($_pkgname-meta)
pkgdesc="Python dead batteries. See PEP 594"
pkgver=3.13.0
pkgrel=1
url="https://github.com/youknowone/python-deadlib"
license=('PSF-2.0')
arch=('any')

depends=(
  'python'
)
declare -A _split_depends
_split_depends["aifc"]="'python-chunk' 'python-audioop'"
_split_depends["asynchat"]="'python-asyncore'"
_split_depends["smtpd"]="'python-asyncore' 'python-asynchat'"
_split_depends["sndhdr"]="'python-aifc'"
_split_depends["sunau"]="'python-audioop'"

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
  pkgname+=("$_modulename")
  _depends+=("$_modulename")

  # create package_* function using here documents
  # Note: variables inside the package function must be escaped
  eval "$(cat <<END
    package_${_modulename}() {
      pkgdesc="Standard library $i redistribution."
      depends+=(${_split_depends[$i]})
      provides+=(
        "python-$i"
      )
      conflicts+=(
        "python-$i"
      )
      groups=(
        $_pkgname
      )
      cd "$_pkgsrc/$i"
      python -m installer --destdir="\$pkgdir/" dist/*.whl
      install -Dm644 LICENSE "\${pkgdir}/usr/share/licenses/${_modulename}/LICENSE"
    }
END
  )"
done

package_python-deadlib-meta() {
  pkgdesc+=" - metapackage"
  depends=("${_depends[@]}")
}
