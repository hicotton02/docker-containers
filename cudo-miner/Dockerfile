FROM ubuntu:18.04
MAINTAINER Keven S.

#global environment settings
ENV RELEASE='stable' \
USERNAME='nobody'

#install wget
RUN apt-get update -y && apt-get install wget -y && \

#get cudo-miner sources
wget -O /etc/apt/trusted.gpg.d/cudo.asc https://download.cudo.org/keys/pgp/apt.asc && \
echo 'deb [arch=amd64] https://download.cudo.org/repo/apt/ ${RELEASE} main' | tee /etc/apt/sources.list.d/cudo.list && \

#install cudo-miner
apt-get update -y && apt-get install cudo-miner-core cudo-miner-service cudo-miner-cli && \


#remove wget
apt-get purge --remove wget -y && \
apt-get autoremove -y

#start cudo-miner
CMD cudominercli enable ${USERNAME}
