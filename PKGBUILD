# Maintainer: Shadow Ma <i at shadow dot ma>

### PATCH AND BUILD OPTIONS
# Set these variables to ANYTHING (yes or no or 1 or 0 or "I like icecream") to enable them
#
_makenconfig=yes		# Tweak kernel options prior to a build via nconfig
_localmodcfg=		# Compile ONLY probed modules
_use_current=		# Use the current kernel's .config file
_BFQ_enable_=yes		# Enable BFQ as the default I/O scheduler
_NUMA_off=yes		# Disable NUMA in kernel config
MAKEFLAGS=

### DOCS

# DETAILS FOR _localmodcfg=
# As of mainline 2.6.32, running with this option will only build the modules that you currently have
# probed in your system VASTLY reducing the number of modules built and the build time to do it.
#
# WARNING - make CERTAIN that all modules are modprobed BEFORE you begin making the pkg!
#
# To keep track of which modules are needed for your specific system/hardware, give my module_db script
# a try: http://aur.archlinux.org/packages.php?ID=41689  Note that if you use my script, this PKGBUILD 
# will auto run the 'sudo modprobed_db reload' for you to probe all the modules you have logged!
#
# More at this wiki page ---> https://wiki.archlinux.org/index.php/Modprobed_db

# DETAILS FOR _use_current=
# Enabling this option will use the .config of the RUNNING kernel rather than the ARCH defaults.
# Useful when the package gets updated and you already went through the trouble of customizing your
# config options.  NOT recommended when a new kernel is released, but again, convenient for package bumps.

# DETAILS FOR _BFQ_enable=
# Alternative I/O scheduler by Paolo.  For more, see: http://algo.ing.unimo.it/people/paolo/disk_sched/

# DRTAILS FOR _NUMA_off=yes
# Since 99.9% of users do not have multiple CPUs but do have multiple cores in one CPU
# see, https://bugs.archlinux.org/task/31187

pkgname=linux-shadow
true && pkgname=(linux-shadow linux-shadow-headers)
_kernelname=-shadow
_srcname=linux-3.7
pkgver=3.7.9
pkgrel=2
arch=('i686' 'x86_64')
url="http://shadow.ma/"
license=('GPL2')
options=('!strip')
#PKGEXT='.pkg.tar' ##less time
_ckpatchversion=1
_ckpatchname="patch-3.7-ck${_ckpatchversion}"
_gcc_patch="kernel-37-gcc47-2.patch"
_bfqpath="http://algo.ing.unimo.it/people/paolo/disk_sched/patches/3.7.0-v6"
_cjkttypath="http://sourceforge.net/projects/cjktty/files/cjktty-for-linux-3.x"
_cjkttyver="3.7.0"
_uksmvernel="0.1.2.2"
_uksmname="v3.7.ge.1"
source=(
        "http://www.kernel.org/pub/linux/kernel/v3.x/${_srcname}.tar.xz"
        "http://www.kernel.org/pub/linux/kernel/v3.x/patch-${pkgver}.xz"
        "http://ck.kolivas.org/patches/3.0/3.7/3.7-ck${_ckpatchversion}/${_ckpatchname}.bz2"
        "http://repo-ck.com/source/gcc_patch/${_gcc_patch}.gz"
        "http://kerneldedup.org/download/uksm/${_uksmvernel}/uksm-${_uksmvernel}-for-${_uksmname}.patch"
	"${_bfqpath}/0001-block-cgroups-kconfig-build-bits-for-BFQ-v6-3.7.patch"
	"${_bfqpath}/0002-block-introduce-the-BFQ-v6-I-O-sched-for-3.7.patch"
	"${_cjkttypath}/cjktty-for-${_cjkttyver}.patch.xz"
        'linux-shadow.preset'
        'change-default-console-loglevel.patch'
        'CVE-2013-1763.patch'
        'fat-3.6.x.patch'
        'http://ck.kolivas.org/patches/bfs/3.0/3.7/3.7-bfs426-427.patch'
        'config'
        'config.x86_64'
        )
        
sha256sums=('60a64d0bf76eeec3355f115c577935757b84629c8c129ce5b8bb02075f6b9458'
            '9f10f53c3cf6dbb4378a3799750b4d0a818f8f5ac112372498d533f4e475cea6'
            '525d31f777e650f0bed604a66e8cbbb2993fe56cd2bc0b36a1dc8f1f8a3d24b7'
            '11183d20458fe662ceffcc2a0caa14200a0ec630e62ec109ad258cb4e005df5e'
            '6dc36a8f33009bb5942dfdfc735661d2306354897b9731b4b0ef2a2615ee2e7d'
            '0551882fb8d401441ff9d4af4bb04c5de84f134a714c66db749d2ce04d7e66c3'
            '3adcd540629f5f0392152ed1740ca36a7c3b1390be14c98e1b9f3c875bfac374'
            '2bcf601d36c943863296ca405bdd0ff40202de47c66f0f26b55f1ba54d3b1baf'
            'bba6e073b31ef3af4fa5dcec66862fe254f2e504f121b784ab1bf6c9ede595ad'
            'b9d79ca33b0b51ff4f6976b7cd6dbb0b624ebf4fbf440222217f8ffc50445de4'
            'd002d25ffb4619da7ac6a71b6f0849d202fb3cd64c3283d5342cc19f6fa360f4'
            '3190cffd7bf4906cb85632764d4c5ec22e8da5ccda4daf1f6f8666c6e2c39d52'
            '231f4d85b1196507b9201d74694701785d8dd5902158e9444b2ba5e96421f0fe'
            '0e2dd29abdaf9a805b653ce796a16ee9a5fdabd749b2b82bbd75bb53bfd64526'
            '0fb3c5d9e1cf0d6cf823a4af1c23626892b661a5f352fd6e1da8c40e5eedff4d')
build() {
	cd "${srcdir}/${_srcname}"

	# add upstream patch
	# msg "Patching source upstream patch set to $pkgver"
	patch -p1 -i "${srcdir}/patch-${pkgver}"

	# add latest fixes from stable queue, if needed
	# http://git.kernel.org/?p=linux/kernel/git/stable/stable-queue.git

	# set DEFAULT_CONSOLE_LOGLEVEL to 4 (same value as the 'quiet' kernel param)
	# remove this when a Kconfig knob is made available by upstream
	# (relevant patch sent upstream: https://lkml.org/lkml/2011/7/26/227)
	patch -Np1 -i "${srcdir}/change-default-console-loglevel.patch"

	# Fix security vulnetability CVE-2013-1763.patch
	# https://bugs.archlinux.org/task/34005
	patch -Np1 -i "${srcdir}/CVE-2013-1763.patch"
	
	# fix cosmetic fat issue
	# https://bugs.archlinux.org/task/32916
	patch -Np1 -i "${srcdir}/fat-3.6.x.patch"

	### Patch source with ck patchset with BFS
	# Fix double name in EXTRAVERSION
	sed -i -re "s/^(.EXTRAVERSION).*$/\1 = /" "${srcdir}/${_ckpatchname}"
	msg "Patching source with ck1 including bfs v0.427"
	patch -Np1 -i "${srcdir}/${_ckpatchname}"
	patch -Np1 -i "${srcdir}/3.7-bfs426-427.patch"

	### Patch source to enable more gcc CPU optimizatons via the make nconfig
	patch -Np1 -i "${srcdir}/${_gcc_patch}"

	#msg "Patching source with BFQ patches"
	for p in $(ls ${srcdir}/000{1,2}-block*.patch); do
		patch -Np1 -i $p
	done

	### Patch source with CJKTTY
	msg "Patching with CJKTTY"
	patch -Np1 -i "${srcdir}/cjktty-for-${_cjkttyver}.patch"
    
	### Patch source with UKSM
	msg "Patching with UKSM"
	patch -Np1 -i "${srcdir}/uksm-${_uksmvernel}-for-${_uksmname}.patch"
    
	### Clean tree and copy ARCH config over
	msg "Running make mrproper to clean source tree"
	make mrproper

	if [ "${CARCH}" = "x86_64" ]; then
		cat "${srcdir}/config.x86_64" > ./.config
	else
		cat "${srcdir}/config" > ./.config
	fi

	### Optionally use running kernel's config
	# code originally by nous; http://aur.archlinux.org/packages.php?ID=40191
	if [ -n "$_use_current" ]; then
		if [[ -s /proc/config.gz ]]; then
			msg "Extracting config from /proc/config.gz..."
			# modprobe configs
			zcat /proc/config.gz > ./.config
		else
			warning "You kernel was not compiled with IKCONFIG_PROC!"
			warning "You cannot read the current config!"
			warning "Aborting!"
			exit
		fi
	fi

	if [ "${_kernelname}" != "" ]; then
		sed -i "s|CONFIG_LOCALVERSION=.*|CONFIG_LOCALVERSION=\"${_kernelname}\"|g" ./.config
		sed -i "s|CONFIG_LOCALVERSION_AUTO=.*|CONFIG_LOCALVERSION_AUTO=n|" ./.config
	fi

	### BFQ to be compiled in but not enabled
	sed -i -e s'/CONFIG_CFQ_GROUP_IOSCHED=y/CONFIG_CFQ_GROUP_IOSCHED=y\nCONFIG_IOSCHED_BFQ=y\nCONFIG_CGROUP_BFQIO=y/' \
		-i -e s'/CONFIG_DEFAULT_CFQ=y/CONFIG_DEFAULT_CFQ=y\n# CONFIG_DEFAULT_BFQ is not set/' ./.config

	### Optionally enable BFQ as the default io scheduler
	if [ -n "$_BFQ_enable_" ]; then
		sed -i -e '/CONFIG_DEFAULT_IOSCHED/ s,cfq,bfq,' \
			-i -e s'/CONFIG_DEFAULT_CFQ=y/# CONFIG_DEFAULT_CFQ is not set\nCONFIG_DEFAULT_BFQ=y/' ./.config
	fi

	# disable NUMA since 99.9% of users do not have multiple CPUs but do have multiple cores in one CPU
	# see, https://bugs.archlinux.org/task/31187
	if [ -n "$_NUMA_off" ]; then
		if [ "${CARCH}" = "x86_64" ]; then
			sed -i -e 's/CONFIG_NUMA=y/# CONFIG_NUMA is not set/' \
				-i -e '/CONFIG_AMD_NUMA=y/d' \
				-i -e '/CONFIG_X86_64_ACPI_NUMA=y/d' \
				-i -e '/CONFIG_NODES_SPAN_OTHER_NODES=y/d' \
				-i -e '/# CONFIG_NUMA_EMU is not set/d' \
				-i -e '/CONFIG_NODES_SHIFT=6/d' \
				-i -e '/CONFIG_NEED_MULTIPLE_NODES=y/d' \
				-i -e '/CONFIG_USE_PERCPU_NUMA_NODE_ID=y/d' \
				-i -e '/CONFIG_ACPI_NUMA=y/d' ./.config
		fi
	fi

	# set extraversion to pkgrel
	sed -ri "s|^(EXTRAVERSION =).*|\1 -${pkgrel}|" Makefile

	# don't run depmod on 'make install'. We'll do this ourselves in packaging
	sed -i '2iexit 0' scripts/depmod.sh

	# get kernel version
	msg "Running make prepare for you to enable patched options of your choosing"
	make prepare

	### Optionally load needed modules for the make localmodconfig
	# See http://aur.archlinux.org/packages.php?ID=41689
	if [ -n "$_localmodcfg" ]; then
		msg "If you have modprobe_db installed, running it in recall mode now"
		if [ -e /usr/bin/modprobed_db ]; then
			[[ ! -x /usr/bin/sudo ]] && echo "Cannot call modprobe with sudo.  Install via pacman -S sudo and configure to work with this user." && exit 1
			sudo /usr/bin/modprobed_db recall
		fi
		msg "Running Steven Rostedt's make localmodconfig now"
		make localmodconfig
	fi

	if [ -n "$_makenconfig" ]; then
		msg "Running make nconfig"
		make nconfig
	fi

	msg "Running make bzImage and modules"
	make ${MAKEFLAGS} LOCALVERSION= bzImage modules
}

package_linux-shadow() {
_Kpkgdesc='The Linux Kernel and modules with BFS, BFQ, cjktty and uksm support.'
pkgdesc="${_Kpkgdesc}"
depends=('coreutils' 'linux-firmware' 'kmod' 'mkinitcpio>=0.7')
optdepends=('crda: to set the correct wireless channels of your country')
provides=("linux-shadow=${pkgver}")
backup=("etc/mkinitcpio.d/linux-shadow.preset")
install=linux-shadow.install


cd "${srcdir}/${_srcname}"

KARCH=x86

# get kernel version
_kernver="$(make LOCALVERSION= kernelrelease)"
_basekernel=${_kernver%%-*}
_basekernel=${_basekernel%.*}

mkdir -p "${pkgdir}"/{lib/modules,lib/firmware,boot}
make LOCALVERSION= INSTALL_MOD_PATH="${pkgdir}" modules_install
cp arch/$KARCH/boot/bzImage "${pkgdir}/boot/vmlinuz-linux-shadow"

# add vmlinux
install -D -m644 vmlinux "${pkgdir}/usr/src/linux-${_kernver}/vmlinux"

# install fallback mkinitcpio.conf file and preset file for kernel
install -D -m644 "${srcdir}/linux-shadow.preset" "${pkgdir}/etc/mkinitcpio.d/linux-shadow.preset"

# set correct depmod command for install
sed \
	-e  "s/KERNEL_NAME=.*/KERNEL_NAME=-shadow/g" \
	-e  "s/KERNEL_VERSION=.*/KERNEL_VERSION=${_kernver}/g" \
	-i "${startdir}/linux-shadow.install"
sed \
	-e "1s|'linux.*'|'linux-shadow'|" \
	-e "s|ALL_kver=.*|ALL_kver=\"/boot/vmlinuz-linux-shadow\"|" \
	-e "s|default_image=.*|default_image=\"/boot/initramfs-linux-shadow.img\"|" \
	-e "s|fallback_image=.*|fallback_image=\"/boot/initramfs-linux-shadow-fallback.img\"|" \
	-i "${pkgdir}/etc/mkinitcpio.d/linux-shadow.preset"

# remove build and source links
rm -f "${pkgdir}"/lib/modules/${_kernver}/{source,build}
# remove the firmware
rm -rf "${pkgdir}/lib/firmware"
# gzip -9 all modules to save 100MB of space
find "${pkgdir}" -name '*.ko' -exec gzip -9 {} \;
# make room for external modules
ln -s "../extramodules-${_basekernel}${_kernelname:--shadow}" "${pkgdir}/lib/modules/${_kernver}/extramodules"
# add real version for building modules and running depmod from post_install/upgrade
mkdir -p "${pkgdir}/lib/modules/extramodules-${_basekernel}${_kernelname:--shadow}"
echo "${_kernver}" > "${pkgdir}/lib/modules/extramodules-${_basekernel}${_kernelname:--shadow}/version"

# Now we call depmod...
depmod -b "$pkgdir" -F System.map "$_kernver"

# move module tree /lib -> /usr/lib
mv "$pkgdir/lib" "$pkgdir/usr"
}

package_linux-shadow-headers() {
_Hpkgdesc='Header files and scripts to build modules for linux-shadow.'
pkgdesc="${_Hpkgdesc}"
depends=('linux-shadow') # added to keep kernel and headers packages matched
provides=("linux-shadow-headers=${pkgver}" "linux-headers=${pkgver}")

install -dm755 "${pkgdir}/usr/lib/modules/${_kernver}"

cd "${pkgdir}/usr/lib/modules/${_kernver}"
ln -sf ../../../src/linux-${_kernver} build

cd "${srcdir}/${_srcname}"
install -D -m644 Makefile \
	"${pkgdir}/usr/src/linux-${_kernver}/Makefile"
install -D -m644 kernel/Makefile \
	"${pkgdir}/usr/src/linux-${_kernver}/kernel/Makefile"
install -D -m644 .config \
	"${pkgdir}/usr/src/linux-${_kernver}/.config"

mkdir -p "${pkgdir}/usr/src/linux-${_kernver}/include"

for i in acpi asm-generic config crypto drm generated linux math-emu \
	media net pcmcia scsi sound trace uapi video xen; do
cp -a include/${i} "${pkgdir}/usr/src/linux-${_kernver}/include/"
	done

	# copy arch includes for external modules
	mkdir -p "${pkgdir}/usr/src/linux-${_kernver}/arch/x86"
	cp -a arch/x86/include "${pkgdir}/usr/src/linux-${_kernver}/arch/x86/"

	# copy files necessary for later builds, like nvidia and vmware
	cp Module.symvers "${pkgdir}/usr/src/linux-${_kernver}"
	cp -a scripts "${pkgdir}/usr/src/linux-${_kernver}"

	# fix permissions on scripts dir
	chmod og-w -R "${pkgdir}/usr/src/linux-${_kernver}/scripts"
	mkdir -p "${pkgdir}/usr/src/linux-${_kernver}/.tmp_versions"

	mkdir -p "${pkgdir}/usr/src/linux-${_kernver}/arch/${KARCH}/kernel"

	cp arch/${KARCH}/Makefile "${pkgdir}/usr/src/linux-${_kernver}/arch/${KARCH}/"

	if [ "${CARCH}" = "i686" ]; then
		cp arch/${KARCH}/Makefile_32.cpu "${pkgdir}/usr/src/linux-${_kernver}/arch/${KARCH}/"
	fi

	cp arch/${KARCH}/kernel/asm-offsets.s "${pkgdir}/usr/src/linux-${_kernver}/arch/${KARCH}/kernel/"

	# add headers for lirc package
	# pci
	for i in bt8xx cx88 saa7134; do
		mkdir -p "${pkgdir}/usr/src/linux-${_kernver}/drivers/media/pci/${i}"
		cp -a drivers/media/pci/${i}/*.h "${pkgdir}/usr/src/linux-${_kernver}/drivers/media/pci/${i}"
	done
	# usb
	for i in cpia2 em28xx pwc sn9c102; do
		mkdir -p "${pkgdir}/usr/src/linux-${_kernver}/drivers/media/usb/${i}"
		cp -a drivers/media/usb/${i}/*.h "${pkgdir}/usr/src/linux-${_kernver}/drivers/media/usb/${i}"
	done
	# i2c
	mkdir -p "${pkgdir}/usr/src/linux-${_kernver}/drivers/media/i2c"
	cp drivers/media/i2c/*.h  "${pkgdir}/usr/src/linux-${_kernver}/drivers/media/i2c/"
	for i in cx25840; do
		mkdir -p "${pkgdir}/usr/src/linux-${_kernver}/drivers/media/i2c/${i}"
		cp -a drivers/media/i2c/${i}/*.h "${pkgdir}/usr/src/linux-${_kernver}/drivers/media/i2c/${i}"
	done

	# add docbook makefile
	install -D -m644 Documentation/DocBook/Makefile \
		"${pkgdir}/usr/src/linux-${_kernver}/Documentation/DocBook/Makefile"

	# add dm headers
	mkdir -p "${pkgdir}/usr/src/linux-${_kernver}/drivers/md"
	cp drivers/md/*.h "${pkgdir}/usr/src/linux-${_kernver}/drivers/md"

	# add inotify.h
	mkdir -p "${pkgdir}/usr/src/linux-${_kernver}/include/linux"
	cp include/linux/inotify.h "${pkgdir}/usr/src/linux-${_kernver}/include/linux/"

	# add wireless headers
	mkdir -p "${pkgdir}/usr/src/linux-${_kernver}/net/mac80211/"
	cp net/mac80211/*.h "${pkgdir}/usr/src/linux-${_kernver}/net/mac80211/"

	# add dvb headers for external modules
	# in reference to:
	# http://bugs.archlinux.org/task/9912
	mkdir -p "${pkgdir}/usr/src/linux-${_kernver}/drivers/media/dvb-core"
	cp drivers/media/dvb-core/*.h "${pkgdir}/usr/src/linux-${_kernver}/drivers/media/dvb-core/"
	# and...
	# http://bugs.archlinux.org/task/11194
	mkdir -p "${pkgdir}/usr/src/linux-${_kernver}/include/config/dvb/"
	[[ -e include/config/dvb/ ]] && cp include/config/dvb/*.h "${pkgdir}/usr/src/linux-${_kernver}/include/config/dvb/" 

	# add dvb headers for http://mcentral.de/hg/~mrec/em28xx-new
	# in reference to:
	# http://bugs.archlinux.org/task/13146
	mkdir -p "${pkgdir}/usr/src/linux-${_kernver}/drivers/media/dvb-frontends/"
	cp drivers/media/dvb-frontends/lgdt330x.h "${pkgdir}/usr/src/linux-${_kernver}/drivers/media/dvb-frontends/"
	cp drivers/media/i2c/msp3400-driver.h "${pkgdir}/usr/src/linux-${_kernver}/drivers/media/i2c/"

	# add dvb headers
	# in reference to:
	# http://bugs.archlinux.org/task/20402
	mkdir -p "${pkgdir}/usr/src/linux-${_kernver}/drivers/media/usb/dvb-usb"
	cp drivers/media/usb/dvb-usb/*.h "${pkgdir}/usr/src/linux-${_kernver}/drivers/media/usb/dvb-usb/"
	mkdir -p "${pkgdir}/usr/src/linux-${_kernver}/drivers/media/dvb-frontends"
	cp drivers/media/dvb-frontends/*.h "${pkgdir}/usr/src/linux-${_kernver}/drivers/media/dvb-frontends/"
	mkdir -p "${pkgdir}/usr/src/linux-${_kernver}/drivers/media/tuners"
	cp drivers/media/tuners/*.h "${pkgdir}/usr/src/linux-${_kernver}/drivers/media/tuners/"

	# add xfs and shmem for aufs building
	mkdir -p "${pkgdir}/usr/src/linux-${_kernver}/fs/xfs"
	mkdir -p "${pkgdir}/usr/src/linux-${_kernver}/mm"
	cp fs/xfs/xfs_sb.h "${pkgdir}/usr/src/linux-${_kernver}/fs/xfs/xfs_sb.h"

	# copy in Kconfig files
	for i in `find . -name "Kconfig*"`; do
		mkdir -p "${pkgdir}"/usr/src/linux-${_kernver}/`echo ${i} | sed 's|/Kconfig.*||'`
		cp ${i} "${pkgdir}/usr/src/linux-${_kernver}/${i}"
	done

	chown -R root.root "${pkgdir}/usr/src/linux-${_kernver}"
	find "${pkgdir}/usr/src/linux-${_kernver}" -type d -exec chmod 755 {} \;

	# strip scripts directory
	find "${pkgdir}/usr/src/linux-${_kernver}/scripts" -type f -perm -u+w 2>/dev/null | while read binary ; do
	case "$(file -bi "${binary}")" in
		*application/x-sharedlib*) # Libraries (.so)
			/usr/bin/strip ${STRIP_SHARED} "${binary}";;
		*application/x-archive*) # Libraries (.a)
			/usr/bin/strip ${STRIP_STATIC} "${binary}";;
		*application/x-executable*) # Binaries
			/usr/bin/strip ${STRIP_BINARIES} "${binary}";;
	esac
done

# remove unneeded architectures
rm -rf "${pkgdir}"/usr/src/linux-${_kernver}/arch/{alpha,arm,arm26,avr32,blackfin,c6x,cris,frv,h8300,hexagon,ia64,m32r,m68k,m68knommu,mips,microblaze,mn10300,openrisc,parisc,powerpc,ppc,s390,score,sh,sh64,sparc,sparc64,tile,unicore32,um,v850,xtensa}
}

# Global pkgdesc and depends are here so that they will be picked up by AUR
pkgdesc='The Linux Kernel and modules with BFS, BFQ, cjktty and uksm support.'
