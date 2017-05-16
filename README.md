# ethminer-cpp docker image

## Description

forked from https://github.com/ethereum/cpp-ethereum/blob/develop/scripts/Dockerfile for miner support

## Usage
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
ethminer-cpp <options>
```

### Example: cpu nanopool
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
ethminer-cpp --cpu --farm-recheck 200 -F http://eth-us-east1.nanopool.org:8888/0xde3a9EDfe3b285659Ae594F00Bfd2EB783D1443c
```

### Example: cpu minergate
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
ethminer-cpp -C -F http://eth.pool.minergate.com:55751/kylemharding@gmail.com --disable-submit-hashrate
```

## Sources

* https://github.com/ethereum/cpp-ethereum
