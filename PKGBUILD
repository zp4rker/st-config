# Maintainer: Jose Riha <jose1711 gmail com>
# Maintainer: Sebastian J. Bronner <waschtl@sbronner.com>
# Contributor: Patrick Jackson <PatrickSJackson gmail com>
# Contributor: Christoph Vigano <mail@cvigano.de>

pkgname=st
pkgver=0.8.5
pkgrel=1
pkgdesc='A simple virtual terminal emulator for X.'
arch=('i686' 'x86_64' 'armv7h' 'aarch64')
license=('MIT')
depends=(libxft)
url=https://st.suckless.org
source=(https://dl.suckless.org/$pkgname/$pkgname-$pkgver.tar.gz
        terminfo.patch
        README.terminfo.rst
        alpha.diff
        blinking_cursor.diff
        delkey.diff
        scrollback.diff
        scrollback-mouse.diff
        dracula.diff
        zp4rker.diff)
sha256sums=('ea6832203ed02ff74182bcb8adaa9ec454c8f989e79232cb859665e2f544ab37'
            'f9deea445a5c6203a0e8e699f3c3b55e27275f17fb408562c4dd5d649edeea23'
            '0ebcbba881832adf9c98ce9fe7667c851d3cc3345077cb8ebe32702698665be2'
            '42e4803ce2a67835f7e533a707a8a28e3804a26ced163145108970b9aee5fb81'
            '1b62ce4dbe25a4dc3e8b350cd5bbaa2097899ff2690774e895bbc8ce0756df5b'
            '946051d123dfe21d8f5ef0f1070c473443f4779dc0bd7edf7c8497f67e325a49'
            'dc7f5223b26fc813d91d4ae35bdaa54d63024cae9f18afd9b3594ba3399dfa55'
            '46ac9bcdbfeb0011533207cb0ab31657a3eb9196da1d0db346e6a9d1fc4b4f76'
            '1ffeaf2c425976d075ac93b2a5658564b91ae3501d2a13192918a92efc20be51'
            '748ea4637e8ed4fda6e691c2062b891b6f805c4d0502becc49054eb3c37d2d57')
_sourcedir=$pkgname-$pkgver
_makeopts="--directory=$_sourcedir"

prepare() {
  patch --directory="$_sourcedir" --strip=0 < terminfo.patch
  patch --directory="$_sourcedir" < dracula.diff
  patch --directory="$_sourcedir" < alpha.diff
  patch --directory="$_sourcedir" < blinking_cursor.diff
  patch --directory="$_sourcedir" < delkey.diff
  patch --directory="$_sourcedir" < scrollback.diff
  patch --directory="$_sourcedir" < scrollback-mouse.diff

  patch --directory="$_sourcedir" < zp4rker.diff

  # This package provides a mechanism to provide a custom config.h. Multiple
  # configuration states are determined by the presence of two files in
  # $BUILDDIR:
  #
  # config.h  config.def.h  state
  # ========  ============  =====
  # absent    absent        Initial state. The user receives a message on how
  #                         to configure this package.
  # absent    present       The user was previously made aware of the
  #                         configuration options and has not made any
  #                         configuration changes. The package is built using
  #                         default values.
  # present                 The user has supplied his or her configuration. The
  #                         file will be copied to $srcdir and used during
  #                         build.
  #
  # After this test, config.def.h is copied from $srcdir to $BUILDDIR to
  # provide an up to date template for the user.
  if [ -e "$BUILDDIR/config.h" ]
  then
    cp "$BUILDDIR/config.h" "$_sourcedir"
  elif [ ! -e "$BUILDDIR/config.def.h" ]
  then
    msg='This package can be configured in config.h. Copy the config.def.h '
    msg+='that was just placed into the package directory to config.h and '
    msg+='modify it to change the configuration. Or just leave it alone to '
    msg+='continue to use default values.'
    echo "$msg"
  fi
  cp "$_sourcedir/config.def.h" "$BUILDDIR"
}

build() {
  make $_makeopts X11INC=/usr/include/X11 X11LIB=/usr/lib/X11
}

package() {
  local installopts='--mode 0644 -D --target-directory'
  local shrdir="$pkgdir/usr/share"
  local licdir="$shrdir/licenses/$pkgname"
  local docdir="$shrdir/doc/$pkgname"
  make $_makeopts PREFIX=/usr DESTDIR="$pkgdir" install
  install $installopts "$licdir" "$_sourcedir/LICENSE"
  install $installopts "$docdir" "$_sourcedir/README"
  install $installopts "$docdir" README.terminfo.rst
  install $installopts "$shrdir/$pkgname" "$_sourcedir/st.info"
}
