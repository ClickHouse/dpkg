# Build it from the root of repository as
# docker build -t dpkg -f docker/Dockerfile .
# For multiple architecture building use the script:

# for arch in arm64 amd64
# do
#   docker buildx build --output=type=tar,dest=../dpkg-deb-${arch}.tar --platform ${arch} -f docker/Dockerfile .
# done

FROM ubuntu:20.04 AS builder

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && \
  apt-get install --yes \
    autoconf \
    autopoint \
    build-essential \
    gettext \
    git \
    libbz2-dev \
    liblzma-dev \
    libncursesw5-dev \
    libtool \
    libz-dev \
    libzstd-dev \
    pkg-config \
  && apt-get clean

COPY . /dpkg

RUN cd /dpkg && \
  ./autogen && \
  ./configure --prefix=/dpkg/out && \
  make -j $(nproc) && \
  make install

FROM ubuntu:20.04

COPY --from=builder /dpkg/out/bin/dpkg-deb /usr/local/bin/dpkg-deb
