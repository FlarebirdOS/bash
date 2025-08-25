pkgname=bash
pkgver=5.3
pkgrel=1
pkgdesc="The GNU Bourne Again shell"
arch=('x86_64')
url="https://www.gnu.org/software/bash/"
license=('GPL-3.0-or-later')
groups=('base')
depends=('glibc' 'readline' 'ncurses')
backup=(etc/{bashrc,profile,shells}
    etc/profile.d/{dircolors,extrapaths,i18n,readline,umask}.sh
    etc/skel/.bash{rc,_profile,_logout})
options=('!lto')
source=(https://ftp.gnu.org/gnu/bash/${pkgname}-${pkgver}.tar.gz
    bashrc
    dircolors.sh
    extrapaths.sh
    i18n.sh
    profile
    readline.sh
    shells
    skel_bash_logout
    skel_bash_profile
    skel_bashrc
    umask.sh)
sha256sums=(62dd49c44c399ed1b3f7f731e87a782334d834f08e098a35f2c87547d5dbb269
    bb71caa18f5a36cce2c2e5a58d214d669369fddf8a771d4ecd1a43162502c59b
    d34963f713aa9e74b79dc8acaacae88008c48f68d741a39773ce599ed2e60c23
    d32661a63c92f2856e75cb105efa46c43ff9b37bb4ad51da5e59e4994aa9d4fe
    c13efccdf5a0959c176811da1de92d01fa90fa0aef162a5e99a9b16f91cc1447
    28c917a731c3fdb4a65748e014b6fa6a3f82c5ef28ccbe1c9a25624fd570b9eb
    db3c9e8e2784233d5bf9a40ec3ee1b379a18de311a1a128a969fa30826c61c33
    77d6556be07e7beef3e405566a25176381cd35291259b9d521f7e87dbb2a2fd6
    b9dadf1fcb36ee6c6831e46a847591c99ab93d9fc562d41cb0fdbbc2685e7793
    5ca4da49f31709f02f30a6acab2636782fc941b85082a98ba41e0e4541b9f0f3
    9481307a8c40e19f315a2da7e6bfcca8ca199598b68b5e34cc5f243f2d76aa4d
    a2ee5dab350cffc0aa68f6ae7b233bef178831b19cfbeba6bd418b7309148d4f)

build() {
    cd ${pkgname}-${pkgver}

    local configure_args=(
        --without-bash-malloc
        --with-installed-readline
        --docdir=/usr/share/doc/${pkgname}-${pkgver}
        ${configure_options}
    )

    ./configure "${configure_args[@]}"

    make
}

package() {
    cd ${pkgname}-${pkgver}

    make DESTDIR=${pkgdir} install

    ln -s bash ${pkgdir}/usr/bin/sh

    install --directory --mode=0755 --owner=root --group=root ${pkgdir}/etc/profile.d

    for file in {dircolors,extrapaths,readline,umask,i18n}.sh
    do
        install -vm644 ${srcdir}/${file} ${pkgdir}/etc/profile.d/${file}
    done

    install -vDm644 ${srcdir}/bashrc  ${pkgdir}/etc/bashrc
    install -vDm644 ${srcdir}/profile ${pkgdir}/etc/profile
    install -vDm644 ${srcdir}/shells  ${pkgdir}/etc/shells

    for file_skel in skel_bash{rc,_profile,_logout}
    do
        install -vDm644 ${srcdir}/${file_skel} ${pkgdir}/etc/skel/.${file_skel#*_}
    done

    dircolors -p > ${pkgdir}/etc/dircolors
}
