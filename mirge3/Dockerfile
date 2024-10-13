# Use Ubuntu 24.04 as the base image
FROM ubuntu:24.04

# Set environment variables for non-interactive timezone setup
ENV DEBIAN_FRONTEND=noninteractive

# Install required packages and update the system
RUN apt-get update && apt-get install -y \
    software-properties-common \
    wget \
    curl \
    gnupg2 \
    ca-certificates \
    build-essential \
    zlib1g-dev \
    libncurses5-dev \
    libbz2-dev \
    liblzma-dev \
    libcurl4-openssl-dev \
    libglib2.0-dev \
    r-base \
    python3-pip \
    python3-dev \
    python3-setuptools \
    samtools \
    autoconf \
    automake \
    libtool \
    flex \
    bison \
    libopenblas-dev \
    && rm -rf /var/lib/apt/lists/*

# Install Miniconda
RUN wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O /miniconda.sh && \
    bash /miniconda.sh -b -p /miniconda && \
    rm /miniconda.sh

# Create a Conda environment with Python 3.8 and install miRge3
RUN /miniconda/bin/conda create -n mirge3-env python=3.8 -y && \
    /miniconda/bin/conda run -n mirge3-env conda install -c conda-forge cython numpy -y && \
    /miniconda/bin/conda run -n mirge3-env conda install -c bioconda mirge3 -y

# Install SWIG from source (for the latest version)
RUN wget https://github.com/swig/swig/archive/refs/tags/v4.1.0.tar.gz && \
    tar -xvzf v4.1.0.tar.gz && \
    cd swig-4.1.0 && \
    ./autogen.sh && \
    ./configure && \
    make && \
    make install && \
    cd .. && rm -rf v4.1.0.tar.gz swig-4.1.0

# Build and install GSL from source with OpenBLAS support
RUN wget ftp://ftp.gnu.org/gnu/gsl/gsl-2.7.tar.gz && \
    tar -xzf gsl-2.7.tar.gz && \
    cd gsl-2.7 && \
    ./configure --prefix=/usr --with-blas=/usr/lib/x86_64-linux-gnu/libopenblas.so && \
    make && \
    make install && \
    cd .. && rm -rf gsl-2.7.tar.gz gsl-2.7



# Install RNA Fold (ViennaRNA package) with Python support, using the manually built GSL and OpenBLAS
RUN ln -s /usr/include/locale.h /usr/include/xlocale.h

RUN /bin/bash -c "source /miniconda/bin/activate mirge3-env && \
    wget https://www.tbi.univie.ac.at/RNA/download/sourcecode/2_4_x/ViennaRNA-2.4.16.tar.gz && \
    tar -xvzf ViennaRNA-2.4.16.tar.gz && \
    cd ViennaRNA-2.4.16 && \
    CFLAGS=\"-fno-lto\" ./configure --with-python=$(which python) && \
    make -j 4 && \
    make install && \
    cd .. && rm -rf ViennaRNA-2.4.16.tar.gz ViennaRNA-2.4.16"

# Ensure the Conda environment is activated by default
ENV PATH="/miniconda/envs/mirge3-env/bin:$PATH"
ENV CONDA_DEFAULT_ENV=mirge3-env
ENV CONDA_PREFIX=/miniconda/envs/mirge3-env

# Override Conda's entrypoint with a more stable conda activation method
RUN echo "source /miniconda/bin/activate mirge3-env" >> ~/.bashrc

# Set the default entrypoint to automatically run miRge3
ENTRYPOINT ["/bin/bash", "-c", "source /miniconda/bin/activate mirge3-env && exec miRge3.0"]
