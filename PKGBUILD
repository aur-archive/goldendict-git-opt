# Maintainer: Chen Zhiqiang <chenzhiqiang at mail dot com>
# Contributor: Eugene Yudin aka Infy <Eugene dot Yudin at gmail dot com>

pkgname=goldendict-git-opt
pkgver=20130122
pkgrel=1
pkgdesc="Feature-rich dictionary lookup program."
arch=('i686' 'x86_64')
url="http://goldendict.org/"
license=('GPL3')
depends=('hunspell' 'libxtst' 'qt' 'zlib' 'libvorbis' 'phonon')
makedepends=('git')
conflicts=()
provides=('goldendict')
replaces=()
source=('goldendict.desktop')
md5sums=('699f8f73be12fe7d2bb30b1e19e19d32')

_gitroot="git://github.com/goldendict/goldendict.git"
_gitname="goldendict"

export PREFIX=/opt/goldendict.org/goldendict

_checkout(){
  cd ${srcdir}
  msg "Connecting to GIT server...."

  if [ -d $_gitname ] ; then
    cd $_gitname && git pull origin
    msg "The local files are updated."
  else
    git clone $_gitroot
  fi

  msg "GIT checkout done or server timeout"
}

build(){
  _checkout
  msg "Starting make..."

  rm -rf "$srcdir/$_gitname-build"
  git clone "$srcdir/$_gitname" "$srcdir/$_gitname-build"
  cd "$srcdir/$_gitname-build"

  #Fixing flags
  echo "QMAKE_CXXFLAGS_RELEASE = $CFLAGS" >> goldendict.pro
  echo "QMAKE_CFLAGS_RELEASE = $CXXFLAGS" >> goldendict.pro

  #Building
  qmake && make
}

package(){
  cd $srcdir/$_gitname-build
  make INSTALL_ROOT=$pkgdir install
  mkdir -p $pkgdir/usr/local/{bin,share}
  ln -nsf $PREFIX/bin/goldendict $pkgdir/usr/local/bin/
  cp $srcdir/goldendict.desktop ${pkgdir}${PREFIX}/share/applications/
  mkdir -p $pkgdir/usr/local/share/applications
  ln -nsf $PREFIX/share/applications/goldendict.desktop $pkgdir/usr/local/share/applications/
  mkdir -p $pkgdir/usr/local/share/pixmaps
  ln -nsf $PREFIX/share/pixmaps/goldendict.png $pkgdir/usr/local/share/pixmaps/
}

