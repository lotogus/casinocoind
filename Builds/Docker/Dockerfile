# casinocoind

# use the ubuntu base image
FROM ubuntu:xenial
MAINTAINER Andre Jochems andre@casinocoin.org

# make sure the package repository is up to date
RUN apt-get update
RUN apt-get -y upgrade

# install the dependencies
RUN apt-get -y install python-software-properties curl git scons ctags cmake pkg-config protobuf-compiler libprotobuf-dev libssl-dev python-software-properties libboost-all-dev

# download source code from official repository
RUN git clone https://github.com/casinocoin/casinocoind.git src; cd src/; git checkout master

# compile
RUN cd src/; scons

# move to root directory and strip
RUN cp src/build/casinocoind casinocoind; strip casinocoind

# make paths
RUN mkdir -p /var/lib/casinocoind/db/nudb
RUN mkdir -p /var/log/casinocoind
RUN mkdir -p /etc/casinocoind
VOLUME /var/lib/casinocoind/

# copy default config
RUN cp src/doc/casinocoind-example.cfg /etc/casinocoind/casinocoind.cfg
RUN cp src/doc/validators-example.txt /etc/casinocoind/validators.txt

# clean source
RUN rm -r src

# set server Timezone to UTC
RUN apt-get -y install tzdata
RUN ln -fs /usr/share/zoneinfo/UTC /etc/localtime && dpkg-reconfigure -f noninteractive tzdata

# expose ports
EXPOSE 17771/udp
EXPOSE 5005/tcp
EXPOSE 4443/tcp

# launch casinocoind when launching the container
ENTRYPOINT ./casinocoind --conf=/etc/casinocoind/casinocoind.cfg
