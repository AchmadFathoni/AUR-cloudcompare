# Original Author: Johannes Sauer <joh.sauer(at)gmail(dot)com>
# Maintainer: Danilo Bargen <aur at dbrgn dot ch>

pkgname=cloudcompare-git
pkgver=2.5.0.r830.g3cd1c04
pkgrel=1
pkgdesc="A 3D point cloud (and triangular mesh) processing software"
arch=('i686' 'x86_64')
url="http://www.danielgm.net/cc/"
license=('GPL2')
depends=('qt4' 'glu' 'mesa')
makedepends=('git' 'cmake' 'pcl')
optdepends=('pcl')
source=("git+https://github.com/cloudcompare/trunk")
md5sums=('SKIP') 


pkgver() {
  cd "$srcdir/trunk"
  git describe --long | sed -r 's/^v//;s/([^-]*-g)/r\1/;s/-/./g'
}

build() {
  cd "$srcdir/trunk"
  
  [[ -d build ]] && rm -r build
  mkdir -p build && cd build
 
  cmake .. \
     	  -DCMAKE_BUILD_TYPE=Release \
		  -DCMAKE_INSTALL_PREFIX=/opt \
	     -DBUILD_QPCL_PLUGIN_DOCUMENTATION=ON \
	  	  -DINSTALL_QPCL_PLUGIN=ON \
		  -DINSTALL_QSRA_PLUGIN=OFF \
		  -DINSTALL_QPOISSON_RECON_PLUGIN=OFF \
		  -DINSTALL_QHPR_PLUGIN=OFF \
		  -DINSTALL_QRANSAC_SD_PLUGIN=OFF \
		  -DINSTALL_QKINECT_PLUGIN=OFF \
		  -DINSTALL_QBLUR_PLUGIN=OFF \
		  -DINSTALL_QEDL_PLUGIN=OFF \
		  -DINSTALL_QPCV_PLUGIN=OFF \
		  -DINSTALL_QDUMMY_PLUGIN=OFF \
		  -DINSTALL_QSSAO_PLUGIN=OFF 

  while true; do
    read -p "Do you want to edit the cmake configuration? [y/N]" yn
 	 yn=${yn:-n}
    case $yn in
       [Yy] )	cmake-gui .. 
		  break;;
       [Nn] )		  break;;
#       * ) echo "Please answer y or n.";;
    esac
  done

  make
}

package() {
  cd "$srcdir/trunk/build"
  make DESTDIR="$pkgdir/" install

  mkdir -p "${pkgdir}/usr/lib"
  mv "${pkgdir}/opt/lib/"* "${pkgdir}/usr/lib"
  rm -r "${pkgdir}/opt/lib/" 

  mkdir -p "${pkgdir}/usr/bin"
  cd "$pkgdir/usr/bin"
  ln -s "../../opt/ccViewer/ccViewer"
  ln -s "../../opt/CloudCompare/CloudCompare"

}
