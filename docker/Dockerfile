ARG L4T_TAG=r36.2.0
ARG HOST_UID=1000
ARG HOST_GID=1000

FROM nvcr.io/nvidia/l4t-base:${L4T_TAG}

ARG HOST_UID
ARG HOST_GID

# Install dependencies
RUN apt-get update && apt-get install -y \
    python3-pip \
    wget \
    git \
    curl \
    libopenblas-dev \
    libhdf5-dev \
    libjpeg-dev \
    libglib2.0-0 \
    libsm6 \
    libxrender1 \
    libxext6 \
    && rm -rf /var/lib/apt/lists/*

# Install Miniconda (Python 3.10)
ENV CONDA_DIR=/opt/conda
RUN wget --quiet https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-aarch64.sh -O miniconda.sh && \
    bash miniconda.sh -b -p $CONDA_DIR && \
    rm miniconda.sh && \
    $CONDA_DIR/bin/conda clean -ya

ENV PATH=$CONDA_DIR/bin:$PATH

# Create conda environment with TensorFlow and other deps
RUN conda create -n tf python=3.10 -y && \
    conda run -n tf pip install --upgrade pip && \
    conda run -n tf pip install numpy scipy matplotlib

# Install TensorFlow compatible with Jetson (wheel spécifique)
RUN conda run -n tf pip install \
    https://developer.download.nvidia.com/compute/redist/jp/v62/tensorflow/tensorflow-2.13.0+nv23.09-cp310-cp310-linux_aarch64.whl

# Install TensorRT Python bindings
RUN conda run -n tf pip install tensorrt nvidia-pyindex nvidia-tensorrt

# Activate environment by default
ENV CONDA_DEFAULT_ENV=tf
ENV PATH=$CONDA_DIR/envs/tf/bin:$PATH
ENV LD_LIBRARY_PATH=$CONDA_DIR/envs/tf/lib:$LD_LIBRARY_PATH

###############################################################################
# ---------- Create a non-root user ------------------------------------------
###############################################################################
RUN groupadd -g ${HOST_GID} dflgroup && \
    useradd -m -u ${HOST_UID} -g dflgroup dfluser && \
    chown -R ${HOST_UID}:${HOST_GID} /workspace

USER dfluser
WORKDIR /workspace

###############################################################################
# ---------- Default command -------------------------------------------------
###############################################################################
CMD ["/bin/bash"]
