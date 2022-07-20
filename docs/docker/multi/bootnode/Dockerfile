FROM ubuntu:16.04
WORKDIR "/root"
ADD ./set_up_file/geth-linux-amd64  ./geth
ADD ./set_up_file/genesis.json  ./genesis.json
ADD ./set_up_file/config.toml  ./config.toml

RUN mkdir -p ./keystore \
&& mkdir -p ./data \
&& chmod 755 ./geth \
&& ./geth init ./genesis.json

CMD exec ./geth --networkid "6660001" --port 30303 --nodekeyhex $nodekeyhex --verbosity 3

EXPOSE 30301/udp
EXPOSE 30303
