FROM ubuntu:16.04

WORKDIR "/root"
ADD ./set_up_file/geth-linux-amd64  ./geth
ADD ./set_up_file/genesis.json  ./genesis.json
ADD ./set_up_file/config.toml  ./config.toml

RUN mkdir -p ./keystore \
&& mkdir -p ./data \
&& chmod 755 ./geth

ARG password
ARG privatekey
RUN echo $password > ./.accountpassword
RUN echo $privatekey > ./.privatekey


RUN ./geth init ./genesis.json
RUN ./geth account import --password ./.accountpassword  ./.privatekey

ENV address=""
ENV bootnodeId=""
ENV bootnodeIp=""

CMD sh -c 'sleep 10; cd /root; ./geth --config config.toml --bootnodes "enode://$bootnodeId@$bootnodeIp:30303?discport=30301" --networkid "6660001" --port 30303 --http --http.addr "0.0.0.0" --http.api "eth,web3,net,admin,debug,personal" --http.corsdomain "*"  --http.port 8545 --syncmode="full"  --allow-insecure-unlock  --verbosity 4'

EXPOSE 8545
EXPOSE 30303
