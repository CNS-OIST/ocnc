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
    libmetis-dev libeigen3-dev  libgsl-dev \
    gsl-bin libreadline-dev python3-all-dev python3-junitparser ipython3 python3-future \
    libmusic-dev music-bin python3-pytest python3-pytest-timeout python3-pytest-xdist python3-pandas \
    gedit firefox desktop-file-utils swig libxrandr-dev libxinerama-dev \
    libxcursor-dev libxi-dev unzip && \
    update-desktop-database

RUN python -m pip install --upgrade --user pip && \
    python -m pip install neuron brian2 pytest jupyterlab build pip-tools \
    lockfile arviz pygame gym[classic_control] meshio ipywidgets ipyvtklink
RUN python -m pip uninstall -y traitlets && python -m pip install traitlets==5.9.0

WORKDIR /tmp
RUN git clone --recursive https://github.com/CNS-OIST/STEPS.git 
WORKDIR /tmp/STEPS
RUN mkdir build
WORKDIR /tmp/STEPS/build
RUN cmake -DCMAKE_INSTALL_PREFIX:PATH=/usr/local -DPython_INSTALL_PREFIX=/usr/local/lib/python3.10/dist-packages .. && make && make install 

WORKDIR /tmp
RUN git clone https://github.com/stan-dev/httpstan.git

WORKDIR /tmp/httpstan
RUN git checkout 4.12.0
RUN make && python -m pip install poetry && python -m poetry build && python -m pip install dist/*.whl
RUN python -m pip install pystan

WORKDIR /tmp
RUN git clone https://github.com/nest/nest-simulator.git
RUN mkdir nest-simulator/build
WORKDIR /tmp/nest-simulator/build
RUN cmake -DCMAKE_INSTALL_PREFIX:PATH=/usr/local .. && make && make install && make installcheck
ENV PYTHONPATH "${PYTHONPATH}:/usr/local/lib/python3.10/site-packages"
ENV PATH "${PATH}:/usr/local/bin"
RUN rm -rf /tmp/* 
RUN mkdir /root/Desktop && \
    echo "[Desktop Entry]\nType=Link\nName=LXTerminal\nIcon=lxterminal\nURL=/usr/share/applications/lxterminal.desktop"\
    >> /root/Desktop/lxterminal.desktop && \
    echo "[Desktop Entry]\nType=Link\nName=ocnc\nIcon=text-html\nURL=https://github.com/CNS-OIST/ocnc"\
    >> /root/Desktop/ocnc_github.desktop