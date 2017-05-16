FROM ubuntu:16.04
MAINTAINER klutchell <kylemharding@gmail.com>
ENV DEBIAN_FRONTEND noninteractive

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

RUN	apt-get update -y && \
	apt-get install -y -qq -o Dpkg::Options::="--force-confnew" --no-install-recommends \
	software-properties-common

RUN	add-apt-repository -y ppa:ethereum/ethereum && \
	add-apt-repository -y ppa:ethereum/ethereum-dev

RUN	apt-get update -y && \
	apt-get install -y -qq -o Dpkg::Options::="--force-confnew" --no-install-recommends \
	ethminer

# clean apt lists to prevent prompts for auto updates
RUN	apt-get clean && \
	rm -rf /var/lib/apt/lists/*

ENTRYPOINT	["/usr/bin/ethminer"]
