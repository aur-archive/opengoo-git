# opengoo-git

# Maintainer: Tybo thibaultporteboeuf_@t_gmail_dot_com

pkgname=opengoo-git
pkgver=20130114
pkgrel=1
pkgdesc="A free and open clone of World Of Goo."
arch=('i686' 'x86_64')
url="http://mandarancio.github.com/OpenGOO/"
license=('GPL3')
groups=()
depends=('box2d' 'libgl' 'freealut' 'openal' 'qt')
makedepends=('git' 'gcc' 'inkscape')
provides=()
conflicts=()
replaces=()
backup=()
options=()
destdir="$pkgdir/opt/$pkgname"
install=
source=(LICENSE
        opengoo.desktop)
noextract=()
md5sums=('608bed987af1b677f1378e29e4878154'
         '2f0bb542306c418db2916499fbaf2453')

_gitroot=git://github.com/Mandarancio/OpenGOO
_gitname=OpenGOO

build() {
  cd "$srcdir"
  msg "Connecting to GIT server...."

  if [[ -d "$_gitname" ]]; then
    cd "$_gitname" && git pull origin
    msg "The local files are updated."
  else
    git clone "$_gitroot" "$_gitname"
  fi

  msg "GIT checkout done or server timeout"
  msg "Starting build..."

  rm -rf "$srcdir/$_gitname-build"
  git clone "$srcdir/$_gitname" "$srcdir/$_gitname-build"
  cd "$srcdir/$_gitname-build"

  cd src

  qmake ./src.pro || return 1
  make || return 1

  cd ..

  msg "Creating Icon from Artwork..."
  # We are using Inkscape for this, as convert fails to keep the transparent background (might be corrected in the future)
  inkscape --export-png=opengoo.png Artworks/black-goo.v1.0.svg || return 1
  
}

package() {
  cd "$srcdir/$_gitname-build"
  mkdir -p "$destdir"
  install -m444     LICENSE                "$destdir"
  install -m444     README                 "$destdir"
  install -m555     OpenGOO                "$destdir"
#  install -m444     *.index                "$destdir"
  mkdir -p "$destdir/resources/sounds"
  mkdir -p "$destdir/resources/music"
  mkdir -p "$destdir/Artworks"
  mkdir -p "$destdir/Levels"
  install -m444     resources/sounds/*.wav "$destdir/resources/sounds/"
  install -m444     resources/music/*.ogg  "$destdir/resources/music/"
  install -m444     resources/*.level      "$destdir/resources/"
  install -m444     Levels/*               "$destdir/Levels/"
  install -m444     Artworks/*              "$destdir/Artworks/"


  # .desktop and icon
  install -Dm644 "$srcdir/opengoo.desktop" "${pkgdir}/usr/share/applications/opengoo.desktop"
  install -Dm644 opengoo.png "${pkgdir}/usr/share/pixmaps/opengoo.png"


  mkdir -p "$pkgdir/usr/bin"
  # Creating launch script
  cat > "$pkgdir/usr/bin/OpenGOO" << EOF
#!/bin/bash
cd /opt/$pkgname
./OpenGOO
EOF

  chmod 555 "$pkgdir/usr/bin/OpenGOO"
}

# vim:set ts=2 sw=2 et:
