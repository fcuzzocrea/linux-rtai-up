This is a debian package to compile an RTAI enabled kernel for the Emutex upboard.
Packaging modification are based on Sebastian Kuzminsky package available at https://github.com/SebKuzminsky/linux-rtai-debian

To compile the debian packages after downloading and installing linux package build dependencies :

* Download 4.4.43 linux source from kernel.org
* Create DFSG linux.orig
        debian/bin/genorig.py ../linux-4.4.43.tar.xz 
* Extract orig and apply patches
        debian/rules orig
* Generate control
        fakeroot debian/rules clean
* Is really needed ?
        fakeroot debian/rules source
        fakeroot debian/rules debian/control-real
* Generate config
        fakeroot make -f debian/rules.gen setup_amd64_none_amd64
        fakeroot make -f debian/rules.gen setup_amd64_rtai_amd64
* Change configuration of the kernel (not advised)
        make -C debian/build/build_amd64_rtai_amd64 menuconfig
* First build kernel binary the build linux-source (needed to manually compile RTAI from rtai.org)
        DEB_BUILD_OPTIONS=parallel=$(($(nproc)*2)) fakeroot make -f debian/rules.gen binary-arch_amd64_rtai
        DEB_BUILD_OPTIONS=parallel=$(($(nproc)*2)) fakeroot make -f debian/rules.gen binary-indep
