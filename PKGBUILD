# Maintainer: Brian Knox <taotetek@gmail.com>


pkgname=rsyslog-git
pkgbasename=rsyslog
pkgver=20120401
pkgrel=2
pkgdesc="development release of rsyslog with liblognorm, mongodb, and redis support"
url="http://www.rsyslog.com/"
arch=('i686' 'x86_64')
license=('GPL3')
depends=('zlib' 'libestr-git' 'libee-git' 'liblognorm-git' 'libmongo-client-git' 'hiredis-git')
replaces=('rsyslog')
makedepends=()
optdepends=()
backup=('etc/rsyslog.conf' \
	'etc/logrotate.d/rsyslog'
	'etc/conf.d/rsyslog')
options=()
source=('rsyslog'
	'rsyslog.logrotate'
	'rsyslog.conf.d')
md5sums=('a18bbcbb6ebdaa13a6ec6d9f3d9eb2da'
         '8065db4bef3061a4f000ba58779f6829'
         '1a0cd4530dd5d1439456d5ae230574d9')
_gitroot="git://git.adiscon.com/git/rsyslog.git"
_gitname="rsyslog-git"
build() {
  cd ${srcdir}
  msg "Connecting to GIT server..."
  if [ -d $_gitname ] ; then
    cd $_gitname && git pull origin
    msg "The local files are updated"
  else
    git clone $_gitroot $_gitname
  fi
  msg "GIT checkout done or server timeout"
  msg "Starting make..."
  cd ${srcdir}/$_gitname
  export PKG_CONFIG_PATH=/usr/local/lib/pkgconfig/
  export LDFLAGS="-lm"
  ./autogen.sh 	--enable-impstats \
		--enable-omhiredis \
		--enable-ommongodb \
		--enable-debug \
		--enable-imttcp \
		--enable-imptcp \
		--enable-omruleset \
		--enable-omprog \
		--enable-mmnormalize \
		--enable-mmjsonparse \
		--prefix=/usr
		
  make
}
package() {
  export PKG_CONFIG_PATH=/usr/local/lib/pkgconfig/
  export LDFLAGS="-lm"
  cd ${srcdir}/$_gitname
  make install DESTDIR=${pkgdir}
  # Install Daemons and Configuration Files
  install -D -m644 ${pkgbasename}.conf ${pkgdir}/etc/${pkgbasename}.conf
  install -D -m755 ${srcdir}/${pkgbasename} ${pkgdir}/etc/rc.d/${pkgbasename}d
  install -D -m644 $srcdir/${pkgbasename}.logrotate ${pkgdir}/etc/logrotate.d/${pkgbasename}
  install -D -m644 ${srcdir}/${pkgbasename}.conf.d ${pkgdir}/etc/conf.d/${pkgbasename}
}
