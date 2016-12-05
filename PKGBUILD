pkgname=ppsspp
pkgver=1.3
pkgrel=1
pkgdesc='A PSP emulator written in C++'
arch=('x86_64')
url='http://www.ppsspp.org/'
license=('GPL2')
depends=('ffmpeg' 'sdl2' 'glu' 'qt5-multimedia')
makedepends=('cmake' 'qt5-tools' 'git')
source=("git+https://github.com/hrydgard/ppsspp.git#tag=v${pkgver}"
        'git+https://github.com/hrydgard/ppsspp-lang.git#commit=cdf4a8d'
        'ppsspp-armips::git+https://github.com/Kingcom/armips.git#commit=1ffab37' 
        'ppsspp.desktop')
md5sums=('SKIP'
         'SKIP'
         'SKIP'
         'c47ea8cda16cf8678aa7a7a1a826ca42')
         
prepare() {
  cd ${pkgname}
  
  for submodule in lang ext/armips; do
    git submodule init ${submodule}
    git config submodule.${submodule}.url ../ppsspp-${submodule#*/}
    git submodule update ${submodule}
  done
  
  sed -i 's|LREL_TOOL = lrelease|LREL_TOOL = lrelease-qt5|' Qt/PPSSPP.pro
}

build() {
  cd ${pkgname}
  
  /usr/lib/qt5/bin/qmake CONFIG+='release' CONFIG+='system_ffmpeg' Qt/PPSSPPQt.pro
  make
}

package() {
  cd ${pkgname}

  install -dm 755 ${pkgdir}/usr/{bin,share/{applications,pixmaps}}
  install -m 755 ppsspp ${pkgdir}/usr/bin/
  install -m 644 assets/unix-icons/icon-512.svg ${pkgdir}/usr/share/pixmaps/ppsspp.svg
  install -m 644 ../ppsspp.desktop ${pkgdir}/usr/share/applications/
}

