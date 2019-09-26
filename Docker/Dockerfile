FROM ubuntu:18.04 as builder
ARG branch=hot_fix_v3

RUN apt-get update && apt-get -y install sudo openssl git ca-certificates vim  build-essential automake libbz2-dev libssl-dev doxygen graphviz libgmp3-dev autotools-dev python2.7 python2.7-dev python3 python3-dev autoconf libtool curl zlib1g-dev ruby libusb-1.0.0-dev libcurl4-gnutls-dev pkg-config libicu-dev && rm -rf /var/lib/apt/lists/*

RUN git clone -b $branch https://github.com/vlbos/ibc_plugin_eos.git --recursive

WORKDIR /ibc_plugin_eos
RUN git remote update && git fetch --tags && git tag ibc-v2.0.2
RUN ./scripts/eosio_build.sh -s EOS -y -P -o Debug \
RUN ./scripts/eosio_install.sh


FROM ubuntu:18.04

RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get -y install libusb-1.0-0 libcurl3-gnutls openssl ca-certificates && rm -rf /var/lib/apt/lists/*
COPY --from=builder /usr/local/lib/* /usr/local/lib/
COPY --from=builder /ibc_plugin_eos/build/bin /opt/eosio/bin
ENV EOSIO_ROOT=/opt/eosio
ENV LD_LIBRARY_PATH /usr/local/lib
ENV PATH /opt/eosio/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
