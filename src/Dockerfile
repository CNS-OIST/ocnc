ARG BASE_CONTAINER=dorowu/ubuntu-desktop-lxde-vnc
FROM $BASE_CONTAINER
LABEL author="Weiliang Chen"

USER root
RUN wget -q -O - https://dl.google.com/linux/linux_signing_key.pub | apt-key add -

RUN apt install -y software-properties-common
RUN add-apt-repository ppa:nest-simulator/nest
RUN apt update
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
    nest
RUN git clone --recursive https://github.com/CNS-OIST/STEPS.git
RUN cd STEPS && mkdir build && cd build && cmake .. && make && make install
RUN rm -rf STEPS && \
    echo "/usr/lib/python3.8/config-3.8-x86_64-linux-gnu" >> /etc/ld.so.conf.d/python-lib.conf \
  && ldconfig
RUN python -m pip install --upgrade --user pip && \
    python -m pip install neuron brian2 pystan==2.19.1.1 pytest && \
    pip install jupyterlab
RUN apt update
RUN apt install -y gedit firefox desktop-file-utils swig libxrandr-dev libxinerama-dev \
    libxcursor-dev libxi-dev unzip && \
    update-desktop-database
RUN python -m pip install lockfile arviz pygame gym[classic_control] meshio ipywidgets ipyvtklink pyvista[all]
RUN mkdir /root/Desktop && \
    echo "[Desktop Entry]\nType=Link\nName=LXTerminal\nIcon=lxterminal\nURL=/usr/share/applications/lxterminal.desktop"\
    >> /root/Desktop/lxterminal.desktop && \
    echo "[Desktop Entry]\nType=Link\nName=ocnc\nIcon=text-html\nURL=https://github.com/CNS-OIST/ocnc"\
    >> /root/Desktop/ocnc_github.desktop
