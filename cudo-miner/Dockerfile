FROM ubuntu:18.04
ARG username

WORKDIR /usr/
COPY cudo-miner-core/data/* .
COPY cudo-miner-cli/data/* .
WORKDIR /etc/
COPY cudo-miner-service/data/* .

CMD cudominercli enable theskaz
