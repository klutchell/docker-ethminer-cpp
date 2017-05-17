# ethminer-cpp docker image

## Description

These docker images include the official development cpp-ethereum linux binaries to provide ethminer support.

While there are forks of ethminer that are focused on GPU mining, I wanted to try my hand at CPU mining
even though it is not recommended nor profitable. I also wanted to use the official cpp-ethereum source if possible.

These images also include `eth` so they could be run as a client/wallet, but for that I would recommend the official
[ethereum/client-cpp](https://hub.docker.com/r/ethereum/client-cpp/) images since they are smaller and better supported.

## Tags

|tag|description|
| --- | --- |
|`latest`|same as `alpine`|
|`alpine`|latest alpine base image and cpp-ethereum development source|
|`ubuntu`|latest ubuntu base image and cpp-ethereum development source|

## Usage
Create some local directories for ethereum data so a new DAG isn't required on every start (optional).
```bash
mkdir ~/.ethereum ~/.ethash ~/.web3
```
Run container with `.ethash` volume mount and other recommended parameters for [ethereum/client-cpp docker images](http://www.ethdocs.org/en/latest/ethereum-clients/cpp-ethereum/installing-binaries/docker.html).
```bash
$ docker run -it --rm \
--name ethminer-cpp \
-p 127.0.0.1:8545:8545 \
-p 0.0.0.0:30303:30303 \
-v ~/.ethereum:/.ethereum \
-v ~/.ethash:/.ethash \
-v ~/.web3:/.web3 \
-e HOME=/ \
--user $(id -u):$(id -g) \
klutchell/ethminer-cpp <options>
```

### Example: CPU mining w/ nanopool
```bash
$ docker run -it --rm \
--name ethminer-cpp \
-p 127.0.0.1:8545:8545 \
-p 0.0.0.0:30303:30303 \
-v ~/.ethereum:/.ethereum \
-v ~/.ethash:/.ethash \
-v ~/.web3:/.web3 \
-e HOME=/ \
--user $(id -u):$(id -g) \
klutchell/ethminer-cpp --cpu --farm-recheck 200 -F http://eth-us-east1.nanopool.org:8888/0xde3a9EDfe3b285659Ae594F00Bfd2EB783D1443c
```

### Example: CPU mining w/ minergate
```bash
$ docker run -it --rm \
--name ethminer-cpp \
-p 127.0.0.1:8545:8545 \
-p 0.0.0.0:30303:30303 \
-v ~/.ethereum:/.ethereum \
-v ~/.ethash:/.ethash \
-v ~/.web3:/.web3 \
-e HOME=/ \
--user $(id -u):$(id -g) \
klutchell/ethminer-cpp -C -F http://eth.pool.minergate.com:55751/kylemharding@gmail.com --disable-submit-hashrate
```

## Sources

* https://github.com/ethereum/cpp-ethereum
* https://github.com/ethereum/cpp-ethereum/tree/develop/scripts/docker

## Documentation

* http://www.ethdocs.org/en/latest/ethereum-clients/cpp-ethereum/index.html
* http://www.ethdocs.org/en/latest/ethereum-clients/cpp-ethereum/building-from-source/linux.html
* http://www.ethdocs.org/en/latest/ethereum-clients/cpp-ethereum/installing-binaries/docker.html