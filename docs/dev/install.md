# Compile and Run

## Download
Download source code via `git`
```
git clone https://github.com/cube-network/Cube.git
```
## Install Golang
Reference: [Go Download and install](https://golang.org/doc/install)

## Compile
```
cd /path/to/cubechain
make geth
```
> If you want to use cross compile, like compiling on `Mac` for `Linux`, use `make build_linux_node` in `/path/to/cubechain/deployment/cross` with docker started


Compilation is completed, the generated binary is in the folder `build/bin`.

## Run
By running `./build/bin/geth --help`, we can get all `option` info. Specific usage can refer to [Command-line Options](https://geth.ethereum.org/docs/interface/command-line-options)

## Deployment

please refer [deployment](/dev/deploy.md)

> SSD is required

## Network
Program will connect into `mainnet` after started. If you want to connect the public testnet, you can add option `--testnet` to command when starting. 

# Using docker images

If you prefer to run a cube-client by docker container, then you can find the official docker images at [cubenetwork/cube-client](https://hub.docker.com/r/cubenetwork/cube-client).

More over, there's a `dockerfile` at the root directory of the [Cube repository](https://github.com/cube-network/Cube), you can build your own images according to your need.
```
docker build -t cubenetwork/cube-client .
```
