# ==================================================================
# module list
# ------------------------------------------------------------------
# python        3.6    (pyenv)
# tensorflow    latest (pip)
# keras         latest (pip)
# pytorch		latest (pip)
# ==================================================================

FROM nvidia/cuda:10.0-cudnn7-devel-ubuntu18.04

LABEL maintainer="chshin@encoredtech.com"

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
#        node-gyp \
#        libssl1.0-dev \
#        nodejs-dev \
#        npm



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
RUN pyenv install 3.6.3 && \
    pyenv global 3.6.3

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
		jupyterlab \
        jupyternotify \
        keras \
        matplotlib \
        numpy \
        pandas \
	    pandas-profiling \
        pipenv \
		plotly \
        pyarrow \
        python-dateutil \
        scikit-learn \
        scipy \
        seaborn \
        tensorflow-gpu \
        tqdm
# ==================================================================
# pytorch
# ------------------------------------------------------------------
 RUN PIP_INSTALL="pip --no-cache-dir install --upgrade" && \
     $PIP_INSTALL \
        future \
        numpy \
        protobuf \
        enum34 \
        pyyaml \
        typing \
        torch \
		&& \
    $PIP_INSTALL \
    "git+https://github.com/pytorch/vision.git" && \
    $PIP_INSTALL \
        torch_nightly -f \
        https://download.pytorch.org/whl/nightly/cu100/torch_nightly.html
# ==================================================================
# Additional Python tools
# ==================================================================
RUN PIP_INSTALL="pip --no-cache-dir install --upgrade" && \
	$PIP_INSTALL \
       setproctitle \
       psutil \
       modin \
       ruamel.yaml
#       jupyterlab_code_formatter \
#       jupyterlab_latex
       

# ==================================================================
# jupyter lab extensions
# ================================================================== 
#RUN jupyter labextension install nbdime-jupyterlab
# --no-build && \
#    jupyter labextension install @krassowski/jupyterlab_go_to_definition && \
#    jupyter labextension install @ryantam626/jupyterlab_code_formatter && \
#    jupyter serverextension enable --py jupyterlab_code_formatter && \
#    jupyter labextension install @jupyter-widgets/jupyterlab-manager --no-build && \
#    jupyter labextension install jupyterlab_bokeh --no-build && \
#    jupyter labextension install bqplot --no-build && \
#    jupyter labextension install @jupyterlab/vega3-extension --no-build && \
#    jupyter labextension install @jupyterlab/git --no-build && \
#    jupyter labextension install @jupyterlab/hub-extension --no-build && \
#    jupyter labextension install jupyterlab_tensorboard --no-build && \
#    jupyter labextension install jupyterlab-kernelspy --no-build && \
#    jupyter labextension install @jupyterlab/plotly-extension --no-build && \
#    jupyter labextension install jupyterlab-chart-editor --no-build && \
#    jupyter labextension install plotlywidget --no-build && \
#    jupyter labextension install @jupyterlab/latex --no-build && \
#    jupyter labextension install jupyter-matplotlib --no-build && \
#    jupyter labextension install jupyterlab-drawio --no-build && \
#    jupyter labextension install jupyter-leaflet --no-build && \
#    jupyter labextension install qgrid --no-build && \
#    jupyter labextension install @mflevine/jupyterlab_html --no-build && \
#    jupyter lab build && \
#        jupyter lab clean && \
#        jlpm cache clean && \
#        npm cache clean --force && \
#        rm -rf $HOME/.node-gyp && \
#        rm -rf $HOME/.local && \
#    


# ==================================================================
# cleanup
# ==================================================================

RUN ldconfig && \
    apt-get clean && \
    apt-get autoremove && \
    rm -rf /var/lib/apt/lists/* /tmp/* ~/*

WORKDIR /root

EXPOSE 8888 6006 8787
