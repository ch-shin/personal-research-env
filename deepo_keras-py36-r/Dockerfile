# ==================================================================
# module list
# ------------------------------------------------------------------
# r             latest (apt)
# python        3.6    (pyenv)
# tensorflow    latest (pip)
# keras         latest (pip)
# ==================================================================

FROM nvidia/cuda:9.0-cudnn7-devel-ubuntu16.04

LABEL maintainer="yim@encoredtech.com"

ENV LC_ALL C.UTF-8
ENV LANG C.UTF-8

RUN rm -rf /var/lib/apt/lists/* \
           /etc/apt/sources.list.d/cuda.list \
           /etc/apt/sources.list.d/nvidia-ml.list

# ==================================================================
# tools
# ------------------------------------------------------------------

RUN APT_INSTALL="apt-get install -y --no-install-recommends" && \
    apt-get update && \
    DEBIAN_FRONTEND=noninteractive $APT_INSTALL \
        # packages for pyenv installation
        make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev \
        wget curl llvm libncurses5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev \
        # useful packages
        ca-certificates \
        cmake \
        font-manager \
        gdebi-core \
        git \
        less \
        software-properties-common \
        ssh \
        vim

# ==================================================================
# r & rstudio
# ------------------------------------------------------------------

RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9 && \
    add-apt-repository 'deb [arch=amd64,i386] https://cran.rstudio.com/bin/linux/ubuntu xenial/' && \
    # r
    APT_INSTALL="apt-get install -y --no-install-recommends" && \
    apt-get update && \
    DEBIAN_FRONTEND=noninteractive $APT_INSTALL \
        r-base \
        r-base-dev

    # rstudio
RUN RSTUDIO_LATEST=$(wget --no-check-certificate -qO- https://s3.amazonaws.com/rstudio-server/current.ver) && \
    [ -z "$RSTUDIO_VERSION" ] && RSTUDIO_VERSION=$RSTUDIO_LATEST || true && \
    wget -q http://download2.rstudio.org/rstudio-server-${RSTUDIO_VERSION}-amd64.deb && \
    gdebi --non-interactive rstudio-server-${RSTUDIO_VERSION}-amd64.deb && \
    rm rstudio-server-*-amd64.deb

RUN echo 'auth-minimum-user-id=0' >> /etc/rstudio/rserver.conf && \
    ## Set up S6 init system
    wget -P /tmp/ https://github.com/just-containers/s6-overlay/releases/download/v1.21.7.0/s6-overlay-amd64.tar.gz && \
    tar xzf /tmp/s6-overlay-amd64.tar.gz -C / && \
    mkdir -p /etc/services.d/rstudio && \
    echo '#!/usr/bin/with-contenv bash \
          \n exec /usr/lib/rstudio-server/bin/rserver --server-daemonize 0' \
          > /etc/services.d/rstudio/run && \
    echo '#!/bin/bash \
          \n rstudio-server stop' \
          > /etc/services.d/rstudio/finish

# ==================================================================
# pyenv & python
# ------------------------------------------------------------------

ENV HOME /root
ENV PYENV_ROOT $HOME/.pyenv
ENV PATH $PYENV_ROOT/shims:$PYENV_ROOT/bin:$PATH
RUN git clone https://github.com/pyenv/pyenv.git $PYENV_ROOT && \
    echo 'export PYENV_ROOT="$HOME/.pyenv"' >> $HOME/.bashrc && \
    echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> $HOME/.bashrc && \
    echo 'if command -v pyenv 1>/dev/null 2>&1; then\n  eval "$(pyenv init -)"\nfi' >> $HOME/.bashrc

    # python
RUN pyenv install 3.6.0 && \
    pyenv global 3.6.0

# ==================================================================
# python packages
# ------------------------------------------------------------------

RUN PIP_INSTALL="pip --no-cache-dir install --upgrade" && \
    $PIP_INSTALL \
        pip \
        setuptools \
        && \
    # tools
    $PIP_INSTALL \
        Cython \
        feather-format \
        flake8 \
        h5py \
        ipywidgets \
        jax \
        jupyter \
        jupyternotify \
        keras \
        matplotlib \
        numpy \
        pandas \
        pipenv \
        pyarrow \
        python-dateutil \
        scikit-learn \
        scipy \
        seaborn \
        tensorflow-gpu \
        tqdm

# ==================================================================
# IRkernel to jupyter
# ------------------------------------------------------------------

RUN Rscript -e 'install.packages("IRkernel")' && \
    Rscript -e 'IRkernel::installspec()'

# ==================================================================
# cleanup
# ------------------------------------------------------------------

RUN ldconfig && \
    apt-get clean && \
    apt-get autoremove && \
    rm -rf /var/lib/apt/lists/* /tmp/* ~/*

WORKDIR /root

EXPOSE 8888 6006 8787
