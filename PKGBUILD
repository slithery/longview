# Maintainer: Slithery <aur at slithery dot uk>

pkgname=longview
pkgver=1.1.4
pkgrel=1
pkgdesc="A system monitoring agent for Linode customers."
arch=('any')
url="https://github.com/linode/$pkgname"
license=('GPL2')
depends=('perl-libwww' 'perl-crypt-ssleay' 'perl-io-socket-inet6' 'perl-linux-distribution' 'perl-json-pp' 'perl-json' 'perl-log-loglite' 'perl-try-tiny' 'perl-dbi')
optdepends=('perl-dbd-mysql: MySQL support')
install=longview.install
source=($url/archive/v$pkgver.tar.gz)
sha256sums=('735811fd9118af91f03a4659d7aaa1b9ccb1c29043ebc97dfeb4b08994a18638')
noextract=(v$pkgver.tar.gz)

package() {
	cd "$pkgdir"

    msg2 "Unpacking source."
    install -d -m755 "opt/linode"
    bsdtar -C "opt/linode/" -xf "$srcdir/v$pkgver.tar.gz" --exclude=debian
    mv "opt/linode/$pkgname-$pkgver" "opt/linode/$pkgname"

    msg2 "Compiling headers."
    h2ph -d "$pkgdir/opt/linode/$pkgname" /usr/include/syscall.h
    h2ph -d "$pkgdir/opt/linode/$pkgname" /usr/include/sys/syscall.h
    h2ph -d "$pkgdir/opt/linode/$pkgname" /usr/include/asm/unistd.h
    h2ph -d "$pkgdir/opt/linode/$pkgname" /usr/include/asm/unistd_32.h
    h2ph -d "$pkgdir/opt/linode/$pkgname" /usr/include/asm/unistd_64.h
    h2ph -d "$pkgdir/opt/linode/$pkgname" /usr/include/bits/wordsize.h
    h2ph -d "$pkgdir/opt/linode/$pkgname" /usr/include/bits/syscall.h

    msg2 "Installing configuration files."
    install -D -m600 "opt/linode/$pkgname/Extras/conf/Apache.conf" "etc/linode/longview.d/Apache.conf"
    install -D -m600 "opt/linode/$pkgname/Extras/conf/MySQL.conf" "etc/linode/longview.d/MySQL.conf"
    install -D -m600 "opt/linode/$pkgname/Extras/conf/Nginx.conf" "etc/linode/longview.d/Nginx.conf"
    touch "etc/linode/longview.key"
    chmod 600 "/etc/linode/longview.key"

    msg2 "Installing service"
    install -D -m644 "opt/linode/$pkgname/Extras/init/longview.service" "usr/lib/systemd/system/longview.service"

    msg2 "Deleting unneeded files."
    rm -rf "opt/linode/$pkgname/Extras/conf"
    rm -rf "opt/linode/$pkgname/Extras/init"
    rm "opt/linode/$pkgname/Extras/install-dependencies.sh"
}
