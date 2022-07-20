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

CMD sh -c 'sleep 5; ./geth --config config.toml --bootnodes "enode://$bootnodeId@$bootnodeIp:30303?discport=30301" --networkid "6660001" --port 30303 --syncmode=full --mine --miner.gasprice "0" --miner.etherbase $address --unlock $address --password ./.accountpassword --http --http.addr "0.0.0.0" --http.api "eth,web3,net,admin,debug,personal" --http.corsdomain "*"  --http.port 8545 --allow-insecure-unlock'

EXPOSE 8545
EXPOSE 30303
