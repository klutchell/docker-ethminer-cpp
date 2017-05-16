# using install instructions from http://www.ethdocs.org/en/latest/ethereum-clients/cpp-ethereum/building-from-source/linux.html

FROM	ubuntu

MAINTAINER	klutchell <kylemharding@gmail.com>

# set workdir
WORKDIR	/

# set frontend noninteractive
ENV	DEBIAN_FRONTEND noninteractive

# update all installed packages
RUN	apt-get update -y && \
	apt-get upgrade -y -o Dpkg::Options::="--force-confnew" --no-install-recommends && \
	apt-get dist-upgrade -y -o Dpkg::Options::="--force-confnew" --no-install-recommends && \
	apt-get -y autoremove && \
	apt-get clean

# avoid hash mismatch errors when behind proxy
RUN	echo 'Acquire::http::Pipeline-Depth 0;' > /etc/apt/apt.conf.d/99fixbadproxy && \
	echo 'Acquire::http::No-Cache true;' >> /etc/apt/apt.conf.d/99fixbadproxy && \
	echo 'Acquire::BrokenProxy    true;' >> /etc/apt/apt.conf.d/99fixbadproxy

# install build dependencies
RUN	apt-get update -y && \
	apt-get install -y -qq -o Dpkg::Options::="--force-confnew" --no-install-recommends \
	ca-certificates \
	locales \
	git \
	build-essential \
	cmake

# install lib dependencies
RUN	apt-get update -y && \
	apt-get install -y -qq -o Dpkg::Options::="--force-confnew" --no-install-recommends \
	libboost-all-dev \
	libleveldb-dev \
	libcurl4-openssl-dev \
	libmicrohttpd-dev \
	libminiupnpc-dev \
	libgmp-dev

# clone source from github
RUN	git clone --recursive https://github.com/ethereum/cpp-ethereum --branch develop --single-branch --depth 1

# build from source
RUN	cd /cpp-ethereum && \
	mkdir /build && \
	cd /build && \
	cmake /cpp-ethereum -DCMAKE_BUILD_TYPE=RelWithDebInfo -DTESTS=Off && \
	make -j $(cat /proc/cpuinfo | grep processor | wc -l) && \
	make install

# clean up
RUN apt-get -y autoremove && \
	apt-get -y clean && \
	rm -rf /var/lib/apt/lists/* && \
	rm -rf /tmp/* && \
	rm -rf /var/tmp/*

# generate and set locale
RUN	locale-gen en_US.UTF-8
ENV	LANG='en_US.UTF-8' LANGUAGE='en_US:en' LC_ALL='en_US.UTF-8'

# default entrypoint
ENTRYPOINT	["/usr/local/bin/ethminer"]
