# Maintainer : Keshav Padram (the.ridikulus.rat) (aatt) (gemmaeiil) (ddoott) (ccoomm)>
# Contributor: Thomas Bächler <thomas@archlinux.org>
# Contributor: Tobias Powalowski <tpowa@archlinux.org>

# _gitroot="git://git.zytor.com/syslinux/syslinux.git"
_gitroot="git://git.kernel.org/pub/scm/boot/syslinux/syslinux.git"
_gitname="syslinux"
_gitbranch="firmware"

__pkgname="syslinux"
_pkgname="${__pkgname}-efi"
pkgname="${_pkgname}-git"

pkgver=6.00.2.g2a81889
pkgrel=1
arch=('x86_64')
pkgdesc="SYSLINUX built for x86_64 UEFI firmwares - GIT Version"
url="http://syslinux.zytor.com/"
license=('GPL2')

makedepends=('git' 'python2' 'nasm' 'gnu-efi-libs')
depends=('dosfstools' 'efibootmgr')

install="${_pkgname}.install"

provides=("${_pkgname}")
conflicts=("${_pkgname}")

options=('!strip' 'docs' '!libtool' 'emptydirs' 'zipman' '!purge' '!makeflags')

source=("${_gitname}::git+${_gitroot}#branch=${_gitbranch}")

sha1sums=('SKIP')

pkgver() {
	cd "${srcdir}/${_gitname}/"
	git describe --always | sed 's|-|.|g' | sed 's|syslinux.||g'
}

build() {
	
	rm -rf "${srcdir}/${_gitname}_build" || true
	cp -r "${srcdir}/${_gitname}" "${srcdir}/${_gitname}_build"
	
	cd "${srcdir}/${_gitname}_build"
	
	## Unset all compiler FLAGS for efi64 build
	unset CFLAGS
	unset CPPFLAGS
	unset CXXFLAGS
	unset LDFLAGS
	unset MAKEFLAGS
	
	## Fix -Werror compile fail
	# sed 's|-Wall|-Wall -Wno-error|g' -i "${srcdir}/${_gitname}_build/mk"/*.mk || true
	
	rm -rf "${srcdir}/${_gitname}_build/BUILD/" || true
	mkdir -p "${srcdir}/${_gitname}_build/BUILD/"
	
	make O="${srcdir}/${_gitname}_build/BUILD" PYTHON="python2" efi64
	echo
	
	make O="${srcdir}/${_gitname}_build/BUILD" PYTHON="python2" efi64 installer
	echo
	
}

package() {
	
	cd "${srcdir}/${_gitname}_build/"
	
	make O="${srcdir}/${_gitname}_build/BUILD" INSTALLROOT="${pkgdir}/" AUXDIR="/usr/lib/syslinux" efi64 install || true
	echo
	
}
