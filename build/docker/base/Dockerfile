FROM nvidia/cuda:11.0-cudnn8-devel-ubuntu18.04
LABEL manteiner="Michele La Porta <michele.laporta@gmail.com>"

ENV DEBIAN_FRONTEND=noninteractive
ENV TZ=Europe/Rome
ENV OPENCV_VERSION="4.4.0"
ENV OpenBLAS_HOME="/opt/OpenBLAS"
ENV TINI_VERSION v0.19.0
ENV SPARK_HOME /opt/spark
ENV EIDOLON_HOME /eidolon

# -- Layer: Image Metadata

ARG build_date

LABEL org.label-schema.build-date=${build_date}
LABEL org.label-schema.name="Apache Spark Standalone Cluster on Docker - Cluster Base Image"
LABEL org.label-schema.description="Cluster base image shipped with Python, Scala and a shared workspace folder"
LABEL org.label-schema.url="https://github.com/michelelaporta/spark-standalone-cluster-on-docker"
LABEL org.label-schema.schema-version="1.0"

# -- Layer: OS + Python + Scala

ARG scala_version
ARG shared_workspace=/opt/workspace
ARG EIDOLON_VERSION=0.1
ARG SPARK_VERSION=3.0.1
ARG SCALA_VERSION=2.12.12
ARG spark_uid=185

RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone \
    && cd $HOME && echo "HOME " + $HOME

RUN dpkg --add-architecture i386 && \ 
        ### Added for libjasper-dev
        echo "deb http://security.ubuntu.com/ubuntu xenial-security main" | tee /etc/apt/sources.list.d/docker.list && \
        apt-get -y update && \
        apt-get -y install \
        curl git libstdc++5:i386 build-essential cmake wget unzip \
        python3-dev python-numpy python3-numpy python3-venv python3-pip \
        rclone liblapack-dev libgl1-mesa-dev \
        pkg-config \
        libatlas-base-dev \
        gfortran \
        libgtk2.0-dev \
        libavcodec-dev \
        libavformat-dev \
        libswscale-dev \
        libavresample-dev \
        libgtk-3-dev \
        libv4l-dev \ 
        libsm6 \
        libxext6 \
        libxrender-dev \
        ffmpeg \
        ## gstreamer
        libgstreamer1.0-dev \
        libgstreamer-plugins-base1.0-dev \
        libgtk2.0-dev \
        libgtk-3-dev \
        libjpeg-dev \
        libpng-dev \
        libtiff-dev \
        libwebp-dev \
        libtbb-dev \
        libtbb2 \
        libtbb-dev \
        libdc1394-22-dev \
        libblas-dev \
        libatlas-base-dev \
        libatlas3-base \
        liblapacke-dev \
        tesseract-ocr \
        libtesseract-dev \
        gfortran \
        libjpeg8-dev \
        libtiff5-dev \
        libhdf5-dev \
        libjasper-dev  \
        libfaac-dev \
        libmp3lame-dev \
        libtheora-dev \
        libvorbis-dev \
        libopencore-amrnb-dev \
        libopencore-amrwb-dev \
        libcanberra-gtk* \
        x264 \
        libxvidcore-dev \
        libx264-dev \
        v4l-utils \
        libxine2-dev \
        libhdf5-dev \
        protobuf-compiler \
        libprotobuf-dev \
        libgoogle-glog-dev \
        libgflags-dev \
        libeigen3-dev \
        zlib1g-dev \
        libncurses5-dev \
        libgdbm-dev \
        libnss3-dev \
        libssl-dev \
        libsqlite3-dev \
        libreadline-dev \
        libffi-dev \
        libbz2-dev \
        netbase \
        ## spark
        openjdk-8-jre \
        libc6 \
        libpam-modules \
        krb5-user \
        libnss3

## test enviroment to see if nvidia driver is available.
# CMD nvidia-smi

## install OpenBLAS from source
# RUN git clone --recursive -b v0.3.10 https://github.com/xianyi/OpenBLAS.git && cd OpenBLAS && make -j$(nproc) && \
#         make PREFIX=${OpenBLAS_HOME} install && rm -rf OpenBLAS

## Install OpenCV
# RUN wget -nv https://github.com/opencv/opencv/archive/${OPENCV_VERSION}.zip && \
#     unzip -q ${OPENCV_VERSION}.zip && \
#     rm ${OPENCV_VERSION}.zip && \
#     wget -nv https://github.com/opencv/opencv_contrib/archive/${OPENCV_VERSION}.zip && \
#     unzip -q ${OPENCV_VERSION}.zip && \
#     rm ${OPENCV_VERSION}.zip

## Prepare build OpenCV
# RUN mkdir /opencv-${OPENCV_VERSION}/build && cd /opencv-${OPENCV_VERSION}/build && cmake \
#         -D CMAKE_BUILD_TYPE=RELEASE \
#         -D CMAKE_INSTALL_PREFIX=/usr/local \
#         -D OPENCV_EXTRA_MODULES_PATH=/opencv_contrib-${OPENCV_VERSION}/modules \
#         -D BUILD_TIFF=on \
#         -D WITH_TBB=ON \
#         -D BUILD_TBB=ON \
#         -D WITH_EIGEN=ON \
#         -D WITH_V4L=ON \
#         -D WITH_LIBV4L=ON \
#         -D WITH_VTK=OFF \
#         -D WITH_OPENGL=ON \
#         -D WITH_LAPACK=ON \
#         -D BUILD_WEBP=OFF \
#         -D OPENCV_ENABLE_NONFREE=ON \
#         -D BUILD_EXAMPLES=OFF \
#         -D INSTALL_C_EXAMPLES=OFF \
#         -D INSTALL_PYTHON_EXAMPLES=ON \
#         -D BUILD_NEW_PYTHON_SUPPORT=ON \
#         -D OPENCV_GENERATE_PKGCONFIG=ON \      
#         -D PYTHON_DEFAULT_EXECUTABLE=/usr/bin/python3 \
#         -D BUILD_opencv_python3=ON \
#         -D BUILD_opencv_python2=OFF \
#         -D WITH_IPP=OFF \
#         -D WITH_FFMPEG=ON \
#         -D CUDA_TOOLKIT_ROOT_DIR=/usr/local/cuda \
#         -D WITH_CUBLAS=ON \
#         -D BUILD_TESTS=OFF \
#         -D BUILD_PERF_TESTS=OFF \
#         -D WITH_CUDA=ON \
#         -D ENABLE_FAST_MATH=ON \
#         -D CUDA_FAST_MATH=ON \
#         -D WITH_CUDNN=ON \
#         -D OPENCV_DNN_CUDA=ON \
#         -D CUDA_ARCH_BIN=5.3,6.0,6.1,7.0,7.5 \
#         -D CUDA_ARCH_PTX="" ..
        # #-D CUDA_GENERATION=Auto


## Install opencv
# RUN cd /opencv-${OPENCV_VERSION}/build && \
#     make -j$(nproc) && \
#     make install && \
#     ldconfig -v

## CLEANUP opencv    
# RUN apt-get clean && \
#     rm -rf /opencv-${OPENCV_VERSION} /opencv_contrib-${OPENCV_VERSION} /var/lib/apt/lists/*


## build python from source
RUN wget -q https://www.python.org/ftp/python/3.8.5/Python-3.8.5.tgz && \
    tar -xf Python-3.8.5.tgz && \
    cd Python-3.8.5 && \
    ./configure --enable-optimizations && \
    make -j$(nproc) build_all && \
    #make -j$(nproc) altinstall
    make -j$(nproc) install

## Install tini
RUN wget -q https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini && chmod 775 tini && mv tini /usr/bin

## Install Scala
RUN wget -q https://downloads.lightbend.com/scala/$SCALA_VERSION/scala-$SCALA_VERSION.deb && \
    dpkg -i scala-$SCALA_VERSION.deb && rm scala-$SCALA_VERSION.deb




RUN mkdir -p ${shared_workspace}/data && \
    mkdir -p /usr/share/man/man1
    #  && \
    # apt-get update -y && \
    # apt-get install -y curl python3 r-base && \
    # ln -s /usr/bin/python3 /usr/bin/python && \
    # curl https://downloads.lightbend.com/scala/${scala_version}/scala-${scala_version}.deb -k -o scala.deb && \
    # apt install -y ./scala.deb && \
    # rm -rf scala.deb /var/lib/apt/lists/*

ENV SCALA_HOME="/usr/bin/scala"
ENV PATH=${PATH}:${SCALA_HOME}/bin:${SPARK_HOME}/bin
ENV SHARED_WORKSPACE=${shared_workspace}

# -- Runtime

VOLUME ${shared_workspace}
CMD ["bash"]
