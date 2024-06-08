ARG BASE_CONTAINER=fredblgr/ubuntu-novnc:22.04
FROM $BASE_CONTAINER
LABEL author="Weiliang Chen"

USER root
RUN wget -q -O - https://dl.google.com/linux/linux_signing_key.pub | apt-key add -

RUN apt install -y software-properties-common
RUN apt-get update
RUN apt install -y build-essential \
    cmake              \
    cython3            \
    g++                \
    gcc                \
    gfortran           \
    git                \
    gmsh               \
    jupyter            \
    jupyter-nbconvert  \
    libboost-dev       \
    libgmsh-dev        \
    libmpich-dev       \
    libopenblas-dev    \
    petsc-dev          \
    pkg-config         \
    python3-matplotlib \
    python3-mpi4py     \
    python3-nose       \
    python3-numpy      \
    python3-pip        \
    python3-scipy      \
    wget               \  
    cmake libboost-all-dev pkg-config libopenblas-dev \
    libmetis-dev libeigen3-dev  libgsl-dev 
RUN pip3 install build pip-tools
WORKDIR /tmp
RUN git clone --recursive https://github.com/CNS-OIST/STEPS.git 
WORKDIR /tmp/STEPS
RUN git checkout ocnc2024 && mkdir build
WORKDIR /tmp/STEPS/build
RUN cmake -DCMAKE_INSTALL_PREFIX:PATH=/usr/local -DPython_INSTALL_PREFIX=/usr/local/lib/python3.10/dist-packages .. && make && make install 
WORKDIR /tmp
RUN rm -rf STEPS 
RUN python -m pip install --upgrade --user pip && \
    python -m pip install neuron brian2 pytest && \
    python -m pip install jupyterlab
RUN python -m pip uninstall -y traitlets && python -m pip install traitlets==5.9.0
RUN apt update
RUN apt install -y gedit firefox desktop-file-utils swig libxrandr-dev libxinerama-dev \
    libxcursor-dev libxi-dev unzip && \
    update-desktop-database
RUN python -m pip install lockfile arviz pygame gym[classic_control] meshio ipywidgets ipyvtklink
RUN mkdir /root/Desktop && \
    echo "[Desktop Entry]\nType=Link\nName=LXTerminal\nIcon=lxterminal\nURL=/usr/share/applications/lxterminal.desktop"\
    >> /root/Desktop/lxterminal.desktop && \
    echo "[Desktop Entry]\nType=Link\nName=ocnc\nIcon=text-html\nURL=https://github.com/CNS-OIST/ocnc"\
    >> /root/Desktop/ocnc_github.desktop
