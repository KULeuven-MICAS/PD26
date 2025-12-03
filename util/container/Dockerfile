# Author: Fanchen Kong <fanchen.kong@kuleuven.be>

# Docker container for PD environment

ARG UBUNTU_VERSION=22.04

#-------------------------------------------------------------------------------
# Stage 1: Tool builder
# Installs generic tool binaries.
#-------------------------------------------------------------------------------
FROM ubuntu:${UBUNTU_VERSION} AS tool-builder
ARG SNITCH_LLVM_VERSION=15.0.0-snitch-0.2.0
# Install APT requirements
RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y \
    # General requirements
    curl wget git tar nano python3 pip gcc build-essential device-tree-compiler libboost-regex-dev libboost-system-dev

# Get the precompiled LLVM toolchain and the spike
RUN wget https://github.com/pulp-platform/llvm-project/releases/download/${SNITCH_LLVM_VERSION}/riscv32-snitch-llvm-ubuntu2204-${SNITCH_LLVM_VERSION}.tar.gz && \
    tar xvf riscv32-snitch-llvm-ubuntu2204-${SNITCH_LLVM_VERSION}.tar.gz && \
    mv riscv32-snitch-llvm-ubuntu2204-${SNITCH_LLVM_VERSION} riscv-llvm && \
    rm -rf riscv32-snitch-llvm-ubuntu2204-${SNITCH_LLVM_VERSION}.tar.gz
ENV PATH="/riscv-llvm/bin:${PATH}"

#-------------------------------------------------------------------------------
# Stage 2: Build Spike
# ------------------------------------------------------------------------------

RUN git clone https://github.com/riscv-software-src/riscv-isa-sim.git && \
    cd riscv-isa-sim && \
    ./configure --prefix=/opt/spike/ && \
    make install -j && \
    cd .. && \
    rm -rf riscv-isa-sim
ENV PATH="/opt/spike/bin:${PATH}"

#-------------------------------------------------------------------------------
# Stage 3: Python Packages
#-------------------------------------------------------------------------------
RUN python3 -m pip config set global.break-system-packages true && \
    pip install hjson mako numpy


