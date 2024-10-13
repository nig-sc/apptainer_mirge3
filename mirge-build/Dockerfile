# Base image: Ubuntu 24.04
FROM ubuntu:24.04

# Set environment variables
ENV DEBIAN_FRONTEND=noninteractive
ENV PATH="$HOME/.local/bin:$PATH"

# Install required dependencies for building Python 3.8 from source
RUN apt-get update && apt-get install -y \
    software-properties-common \
    wget \
    build-essential \
    libssl-dev \
    zlib1g-dev \
    libbz2-dev \
    libreadline-dev \
    libsqlite3-dev \
    curl \
    llvm \
    libncurses5-dev \
    libncursesw5-dev \
    xz-utils \
    tk-dev \
    libffi-dev \
    liblzma-dev \
    gfortran \
    libopenblas-dev \
    libatlas-base-dev \
    python3-dev \
    && apt-get clean

# Download and build Python 3.8 from source
RUN wget https://www.python.org/ftp/python/3.8.16/Python-3.8.16.tgz \
    && tar xzf Python-3.8.16.tgz \
    && cd Python-3.8.16 \
    && ./configure --enable-optimizations \
    && make -j$(nproc) \
    && make altinstall

# Install pip and distutils for Python 3.8
RUN python3.8 -m ensurepip --upgrade \
    && python3.8 -m pip install --upgrade pip setuptools wheel

# Create a virtual environment using Python 3.8 and install numpy, scipy, miRge-build
RUN python3.8 -m venv /opt/venv \
    && /opt/venv/bin/pip install --upgrade pip setuptools wheel \
    && /opt/venv/bin/pip install numpy==1.19.5 scipy==1.9.3 \
    && /opt/venv/bin/pip install miRge-build


# Ensure the virtual environment is used by default
ENV PATH="/opt/venv/bin:$PATH"

# Entry point (Optional: for interactive shell)
CMD ["/bin/bash
