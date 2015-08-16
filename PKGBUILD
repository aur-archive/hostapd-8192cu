# Maintainer:  Xiaoxiao Pu <i@xiaoxiao.im>

pkgname=hostapd-8192cu
pkgver=0.8_rtw_r7475.20130812_beta
pkgrel=3
pkgdesc="IEEE 802.11 AP, IEEE 802.1X/WPA/WPA2/EAP/RADIUS Authenticator, compatible with Realtek RTL8188CUS (8188C, 8192C) chipset wireless cards"
arch=('i686' 'x86_64' 'armv6h' 'armv7h')
url="http://www.realtek.com.tw/"
license=('GPL2')
depends=('glibc')
provides=('hostapd')
conflicts=('hostapd')
install=hostapd.install
source=('https://github.com/XiaoxiaoPu/hostapd-8192cu/raw/master/RTL8188C_8192C_USB_linux_v4.0.2_9000.20130911.zip'
        'hostapd.service'
        'config')
sha256sums=('14f5775cfa4caf494b231a9f16ccaa2400d9c66db8443f970ec0b905971f69e8'
            '50ab35d587d72b86ff3fc6e9542d0c8a3e63d60e7fb66b10a87ff006f0d96f07'
            'c1c33c6b990db3ff7cd79500ee33d2b8f9c9f44e0b65abe399bb0748fdeb1344')

build() {
	cd "${srcdir}/RTL8188C_8192C_USB_linux_v4.0.2_9000.20130911"
	cd wpa_supplicant_hostapd
	tar -zxf wpa_supplicant_hostapd-0.8_rtw_r7475.20130812.tar.gz \
	    -C "${srcdir}/"
	cd "${srcdir}/wpa_supplicant_hostapd-0.8_rtw_r7475.20130812"
	cd hostapd
	cp "${srcdir}/config" .config
	make
}

package() {
	# Systemd unit
	install -Dm644 "${srcdir}/hostapd.service" \
	    "${pkgdir}/usr/lib/systemd/system/hostapd.service"

	cd "${srcdir}/wpa_supplicant_hostapd-0.8_rtw_r7475.20130812"
	cd hostapd

	# Binaries
	install -Dm755 hostapd "${pkgdir}/usr/bin/hostapd"
	install -Dm755 hostapd_cli "${pkgdir}/usr/bin/hostapd_cli"

	# Configuration
	mkdir -p "${pkgdir}/etc/hostapd"
	mkdir -p "${pkgdir}/usr/share/doc/hostapd"
	install -m644 hlr_auc_gw.milenage_db hostapd.accept \
	    hostapd.conf hostapd.deny hostapd.eap_user \
	    hostapd.radius_clients hostapd.sim_db \
	    hostapd.vlan hostapd.wpa_psk wired.conf \
	    "${pkgdir}/usr/share/doc/hostapd"

	# Man pages
	install -Dm644 hostapd_cli.1 "${pkgdir}/usr/share/man/man1/hostapd_cli.1"
	install -Dm644 hostapd.8 "${pkgdir}/usr/share/man/man8/hostapd.8"

	# Example configuration
	cd "${srcdir}/RTL8188C_8192C_USB_linux_v4.0.2_9000.20130911"
	cd wpa_supplicant_hostapd
	install -m644 rtl_hostapd_2G.conf rtl_hostapd_5G.conf \
	    "${pkgdir}/etc/hostapd/"
}
