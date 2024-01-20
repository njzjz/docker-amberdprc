FROM almalinux:8

# install CUDA 11.8
RUN dnf -y install 'dnf-command(config-manager)' \
    && dnf config-manager --add-repo http://developer.download.nvidia.com/compute/cuda/repos/rhel8/x86_64/cuda-rhel8.repo \
    && dnf -y install cuda-libraries-11-8 cuda-nvtx-11-8 libcublas-11-8 cuda-compat-11-8 cuda-cudart-11-8 libcudnn8

ENV AMBERHOME="/amber"

RUN dnf --enablerepo=powertools -y install tcsh make \
        gcc gcc-gfortran gcc-c++ \
        which flex bison patch bc \
        libXt-devel libXext-devel \
        perl perl-ExtUtils-MakeMaker util-linux wget \
        bzip2 bzip2-devel zlib-devel tar \
        openmpi-devel cmake git \
        openblas-devel

# build xtb
RUN wget https://codeload.github.com/grimme-lab/xtb/zip/0fb151dceb19b8df9f6012789cf7d2dd6e4a6cd2 -O xtb.zip \
    && unzip xtb.zip \
    && mkdir xtb-0fb151dceb19b8df9f6012789cf7d2dd6e4a6cd2/build \
    && cd xtb-0fb151dceb19b8df9f6012789cf7d2dd6e4a6cd2/build \
    && cmake .. -DCMAKE_INSTALL_PREFIX=${AMBERHOME} \
                -DINSTALL_MODULES=ON \
    && cmake --build . --target install \
    && cd ../.. \
    && rm -rf xtb.zip xtb-0fb151dceb19b8df9f6012789cf7d2dd6e4a6cd2

# download deepmd-kit
RUN wget https://github.com/deepmodeling/deepmd-kit/releases/download/v2.2.7/libdeepmd_c.tar.gz -O libdeepmd_c.tar.gz \
    && tar -xzf libdeepmd_c.tar.gz -C ${AMBERHOME} --strip-components=1 \
    && rm libdeepmd_c.tar.gz

# compile AMBER
RUN wget https://public.njzjz.win/AmberTools23.10jan24.tar.bz2 -O AmberTools23.10jan24.tar.bz2 \
    && tar -xjf AmberTools23.10jan24.tar.bz2 \
    && rm AmberTools23.10jan24.tar.bz2 \
    && cd amber22_src/build \
    && cmake .. \
             -DCMAKE_INSTALL_PREFIX=${AMBERHOME} \
             -DCMAKE_PREFIX_PATH="/usr/lib64/openmpi;${AMBERHOME}" \
             -DCOMPILER=GNU \
             -DMPI=ON \
             -DCUDA=OFF \
             -DDOWNLOAD_MINICONDA=OFF \
             -DBUILD_PYTHON=OFF \
             -DUSE_XTB=ON \
             -DUSE_DEEPMDKIT=ON \
    && cmake --build . --target install \
    && cd ../.. \
    && rm -rf amber22_src

ENV PATH="${AMBERHOME}/bin:${PATH}"
ENV LD_LIBRARY_PATH="${AMBERHOME}/lib:${AMBERHOME}/lib64:${LD_LIBRARY_PATH}"
ENV PERL5LIB="${AMBERHOME}/lib/perl:${PERL5LIB}"
ENV QUICK_BASIS="${AMBERHOME}/AmberTools/src/quick/basis"

LABEL org.opencontainers.image.source=https://github.com/njzjz/docker-amberdprc
LABEL org.opencontainers.image.title="Amber DPRc"
LABEL org.opencontainers.image.description="Amber DPRc"
LABEL org.opencontainers.image.licenses=LGPL-3.0-or-later
LABEL org.opencontainers.image.authors="Jinzhe Zeng <jinzhe.zeng@rutgers.edu>"
LABEL org.opencontainers.image.url=https://github.com/njzjz/docker-amberdprc
LABEL org.opencontainers.image.documentation=https://github.com/njzjz/docker-amberdprc
LABEL org.opencontainers.image.vendor="Jinzhe Zeng"

CMD ["/bin/bash"]