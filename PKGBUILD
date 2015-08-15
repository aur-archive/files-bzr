# Maintainer: Ner0

pkgname=files-bzr
pkgver=1003
pkgrel=1
pkgdesc="A simple, powerful and sexy file manager from elementary (Marlin fork)"
arch=('i686' 'x86_64')
url="https://launchpad.net/pantheon-files"
license=('GPL3')
groups=('pantheon')
depends=('granite-bzr>=424' 'glib2' 'pango' 'gtk3' 'libgee' 'sqlite3' 'dbus-glib' 'gtk-update-icon-cache'
         'libnotify' 'gnome-icon-theme' 'dconf' 'cairo' 'atk' 'desktop-file-utils')
makedepends=('bzr' 'vala' 'cmake' 'pkg-config')
optdepends=('tumbler: for generating thumbnails'
            'contractor-bzr: plugins integration'
            'pantheon-files-plugin-dropbox-bzr: Dropbox integration')
conflicts=('marlin-bin' 'marlin-bzr')
install=$pkgname.install

_bzrtrunk=lp:pantheon-files
_bzrmod=files

_UNITY=OFF
[ -f /usr/lib/libunity.so.[0-9] ] && _UNITY=ON

build() {
  msg "Connecting to Bazaar server...."

  if [ -d $_bzrmod ]; then
    cd $_bzrmod && bzr pull $_bzrtrunk -r $pkgver && cd ..
    msg "The local files are updated."
  else
    bzr branch $_bzrtrunk $_bzrmod -r $pkgver
  fi

  msg "BZR checkout done or server timeout"
  msg "Starting make..."

  rm -rf $_bzrmod-build
  cp -r $_bzrmod $_bzrmod-build
  cd $_bzrmod-build
  rm -rf build/

  # Fix vala-018 build
  sed -i 's/\(#define FM_TYPE_DIRECTORY_VIEW fm_directory_view_get_type()\)/\#define FM_DIRECTORY_TYPE_VIEW fm_directory_view_get_type()\n\1/' src/fm-directory-view.h

  mkdir build
  cd build

  cmake .. -DCMAKE_INSTALL_PREFIX=/usr -DGSETTINGS_COMPILE=OFF -DICON_UPDATE=OFF -DWITH_UNITY=${_UNITY}

  make ${MAKEFLAGS}
}

package() {
  cd $_bzrmod-build/build
  make GCONF_DISABLE_MAKEFILE_SCHEMA_INSTALL=1 DESTDIR="$pkgdir/" install
}
