# Contributor: Thomas Baechler <thomas@archlinux.org>
# Maintainer: Philip Müller <philm[at]manjaro[dog]org>
# Maintainer: Roland Singer <roland[at]manjaro[dog]org>

_pkgbasename=nvidia-utils
pkgbase=lib32-$_pkgbasename
pkgname=('lib32-nvidia-utils' 'lib32-opencl-nvidia')
pkgver=396.24
pkgrel=2
epoch=1
arch=('x86_64')
url="http://www.nvidia.com/"
license=('custom')
options=('!strip')

_pkg="NVIDIA-Linux-${arch}-${pkgver}"
durl="http://us.download.nvidia.com/XFree86/Linux-x86_64/${pkgver}"
#durl="http://developer.download.nvidia.com/assets/opengl/369.00"
source=("${durl}/NVIDIA-Linux-x86_64-${pkgver}.run")
sha256sums=('59bb112b17ca72cd15fed4e521d4b52b0e9f5b8b13a95b36521f8eda978b568e')

create_links() {
    # create soname links
    for _lib in $(find "${pkgdir}" -name '*.so*' | grep -v 'xorg/'); do
        _soname=$(dirname "${_lib}")/$(readelf -d "${_lib}" | grep -Po 'SONAME.*: \[\K[^]]*' || true)
        _base=$(echo ${_soname} | sed -r 's/(.*).so.*/\1.so/')
        [[ -e "${_soname}" ]] || ln -s $(basename "${_lib}") "${_soname}"
        [[ -e "${_base}" ]] || ln -s $(basename "${_soname}") "${_base}"
    done
}

build() {
    sh ${_pkg}.run --extract-only
}

package_lib32-opencl-nvidia() {
    pkgdesc="OpenCL implemention for NVIDIA (32-bit)"
    depends=('lib32-zlib' 'lib32-gcc-libs')
    optdepends=('opencl-headers: headers necessary for OpenCL development')
    provides=('lib32-opencl-driver' "lib32-opencl-nvidia=$pkgver")
    cd "${_pkg}"

    # OpenCL
    install -D -m755 "32/libnvidia-compiler.so.${pkgver}" "${pkgdir}/usr/lib32/libnvidia-compiler.so.${pkgver}"
    install -D -m755 "32/libnvidia-opencl.so.${pkgver}" "${pkgdir}/usr/lib32/libnvidia-opencl.so.${pkgver}" 

    create_links

    mkdir -p "${pkgdir}/usr/share/licenses"
    ln -s $_pkgbasename "${pkgdir}/usr/share/licenses/lib32-opencl-nvidia"
}


package_lib32-nvidia-utils() {
    pkgdesc="NVIDIA drivers utilities (32-bit)"
    depends=('lib32-zlib' 'lib32-gcc-libs' 'lib32-libglvnd' 'mhwd' 'nvidia-utils')
    optdepends=('lib32-opencl-nvidia')
    provides=('lib32-vulkan-driver' 'lib32-opengl-driver' 'lib32-nvidia-libgl' "lib32-nvidia-utils=$pkgver")
    conflicts=('lib32-nvidia-libgl')
    replaces=('lib32-nvidia-libgl')
    install="${pkgname}.install"

    cd "${_pkg}"

    # GLX extension module for X
    #install -D -m755 "32/libglx.so.${pkgver}" "${pkgdir}/usr/lib32/nvidia/xorg/modules/extensions/libglx.so.${pkgver}"
    #ln -s "libglx.so.${pkgver}" "${pkgdir}/usr/lib32/nvidia/xorg/modules/extensions/libglx.so"	# X doesn't find glx otherwise
    install -D -m755 "32/libGLX.so.0" "${pkgdir}/usr/lib32/nvidia/xorg/modules/extensions/libglx.so.0"
    ln -s "libglx.so.0" "${pkgdir}/usr/lib32/nvidia/xorg/modules/extensions/libglx.so"	# X doesn't find glx otherwise
    install -D -m755 "32/libGLX_nvidia.so.${pkgver}" "${pkgdir}/usr/lib32/libGLX_nvidia.so.${pkgver}"
    #ln -s "libGLX_nvidia.so.${pkgver}" "${pkgdir}/usr/lib32/libGLX_indirect.so.0"

    # Wayland
    #install -D -m755 "32/libnvidia-egl-wayland.so.1.0.3" "${pkgdir}/usr/lib32/libnvidia-egl-wayland.so.1.0.3"
    #ln -s "libnvidia-egl-wayland.so.1.0.3" "${pkgdir}/usr/lib32/libnvidia-egl-wayland.so.1"

    # OpenGL libraries
    install -D -m755 "32/libGL.so.1.7.0" "${pkgdir}/usr/lib32/nvidia/libGL.so.1.7.0"
    #install -D -m755 "32/libGLdispatch.so.0" "${pkgdir}/usr/lib32/libGLdispatch.so.0"
    install -D -m755 "32/libEGL.so.1.1.0" "${pkgdir}/usr/lib32/nvidia/libEGL.so.1.1.0"
    install -D -m755 "32/libEGL_nvidia.so.${pkgver}" "${pkgdir}/usr/lib32/libEGL_nvidia.so.${pkgver}"
    #install -D -m755 "32/libGLESv1_CM.so.1.2.0" "${pkgdir}/usr/lib32/nvidia/libGLESv1_CM.so.1.2.0"
    install -D -m755 "32/libGLESv1_CM_nvidia.so.${pkgver}" "${pkgdir}/usr/lib32/libGLESv1_CM_nvidia.so.${pkgver}"
    #install -D -m755 "32/libGLESv2.so.2.1.0" "${pkgdir}/usr/lib32/nvidia/libGLESv2.so.2.1.0"
    install -D -m755 "32/libGLESv2_nvidia.so.${pkgver}" "${pkgdir}/usr/lib32/libGLESv2_nvidia.so.${pkgver}"
    #install -D -m755 "32/libOpenGL.so.0" "${pkgdir}/usr/lib32/libOpenGL.so.0"

    # OpenGL core library
    install -D -m755 "32/libnvidia-glcore.so.${pkgver}" "${pkgdir}/usr/lib32/libnvidia-glcore.so.${pkgver}"
    install -D -m755 "32/libnvidia-eglcore.so.${pkgver}" "${pkgdir}/usr/lib32/libnvidia-eglcore.so.${pkgver}"
    install -D -m755 "32/libnvidia-glsi.so.${pkgver}" "${pkgdir}/usr/lib32/libnvidia-glsi.so.${pkgver}"
    install -D -m755 "32/libnvidia-glvkspirv.so.$pkgver" "${pkgdir}/usr/lib32/libnvidia-glvkspirv.so.${pkgver}"

    # misc
    install -D -m755 "32/libnvidia-ifr.so.${pkgver}" "${pkgdir}/usr/lib32/libnvidia-ifr.so.${pkgver}"
    install -D -m755 "32/libnvidia-fbc.so.${pkgver}" "${pkgdir}/usr/lib32/libnvidia-fbc.so.${pkgver}"
    install -D -m755 "32/libnvidia-encode.so.${pkgver}" "${pkgdir}/usr/lib32/libnvidia-encode.so.${pkgver}"
    #install -D -m755 "32/libnvidia-cfg.so.${pkgver}" "${pkgdir}/usr/lib32/libnvidia-cfg.so.${pkgver}"
    install -D -m755 "32/libnvidia-ml.so.${pkgver}" "${pkgdir}/usr/lib32/libnvidia-ml.so.${pkgver}"
    #install -D -m755 "32/libnvidia-wfb.so.${pkgver}" "${pkgdir}/usr/lib32/libnvidia-wfb.so.${pkgver}"

    # VDPAU
    install -D -m755 "32/libvdpau_nvidia.so.${pkgver}" "${pkgdir}/usr/lib32/vdpau/libvdpau_nvidia.so.${pkgver}"

    # nvidia-tls library
    install -D -m755 "32/libnvidia-tls.so.${pkgver}" "${pkgdir}/usr/lib32/libnvidia-tls.so.${pkgver}"
    install -D -m755 "32/tls/libnvidia-tls.so.${pkgver}" "${pkgdir}/usr/lib32/tls/libnvidia-tls.so.${pkgver}"

    # CUDA
    install -D -m755 "32/libcuda.so.${pkgver}" "${pkgdir}/usr/lib32/libcuda.so.${pkgver}"
    install -D -m755 "32/libnvcuvid.so.${pkgver}" "${pkgdir}/usr/lib32/libnvcuvid.so.${pkgver}"

    # PTX JIT Compiler (Parallel Thread Execution (PTX) is a pseudo-assembly language for CUDA)
    install -D -m755 "32/libnvidia-ptxjitcompiler.so.${pkgver}" "${pkgdir}/usr/lib32/libnvidia-ptxjitcompiler.so.${pkgver}"

    # Fat (multiarchitecture) binary loader
    install -D -m755 "32/libnvidia-fatbinaryloader.so.${pkgver}" "${pkgdir}/usr/lib32/libnvidia-fatbinaryloader.so.${pkgver}"

    create_links

    mkdir -p "${pkgdir}/usr/share/licenses"
    ln -s $_pkgbasename "${pkgdir}/usr/share/licenses/${pkgname}"
}

