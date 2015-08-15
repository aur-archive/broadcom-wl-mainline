# Adapted from https://aur.archlinux.org/packages/br/broadcom-wl/PKGBUILD
# Maintainer: Christophe Gueret <christophe.gueret@gmail.com>

pkgname=broadcom-wl-mainline
pkgver=5.100.82.112
pkgrel=1
pkgdesc="Broadcom 802.11abgn hybrid Linux networking device driver (linux-mainline)"
read _ _kernver < <(file -b "/boot/vmlinuz-linux-mainline" | grep -o 'version [^ ]\+')
_kernpkgver=$(pacman -Q linux-mainline|cut -d' ' -f 2)
_extramodules=extramodules-3.6-mainline
url='http://www.broadcom.com/support/802.11/linux_sta.php'
arch=('i686' 'x86_64')
license=('custom')
depends=('linux-mainline')

[[ $CARCH = x86_64 ]] && ARCH=x86_64 || ARCH=x86_32
source=("http://www.broadcom.com/docs/linux_sta/hybrid-portsrc_${ARCH}-v${pkgver//./_}.tar.gz"
        'modprobe.d'
        'license.patch'
        'user-ioctl.patch'
        'linux-recent.patch')
sha1sums=('01aa32f9e85621253a3f15cf4361bb80d41da3e8'
          '89bf92286ede30dd85304c6c4e42e89cfdc0f60a'
          'ea7b67982ddc0f56fd3becb9914fd4458fe7d373'
          'e0366dd15ea5b25aae0f61bec6be7a203dfca7e4'
          'dabb245b93925a449cd7886b8600f30aa16e580e')
[[ $CARCH = x86_64 ]] && sha1sums[0]='5bd78c20324e6a4aa9f3fafdc6f0155e884d5131'

backup=('etc/modprobe.d/broadcom-wl.conf')
install=install



build() {
	cd "${srcdir}"

	patch -p1 -i linux-recent.patch
	patch -p1 -i user-ioctl.patch
	patch -p1 -i license.patch

	make -C /usr/lib/modules/"${_kernver}"/build M=`pwd`
}

package() {
	cd "${srcdir}"

	install -D -m 755 wl.ko "${pkgdir}/usr/lib/modules/${_extramodules}/wl.ko"
	gzip "${pkgdir}/usr/lib/modules/${_extramodules}/wl.ko"

	install -D -m 644 lib/LICENSE.txt "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
	install -D -m 644 modprobe.d "${pkgdir}"/etc/modprobe.d/broadcom-wl.conf
}


