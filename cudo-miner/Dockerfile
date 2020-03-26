FROM ubuntu:18.04
ARG username

WORKDIR /usr/
COPY cudo-miner-core/data/* .
COPY cudo-miner-cli/data/* .
WORKDIR /etc/
COPY cudo-miner-service/data/* .

#start cudo-miner
CMD cudominercli enable $username 
