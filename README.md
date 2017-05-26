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

## Basic Usage

Create some local directories for ethereum data so a new DAG isn't required on every start (optional).
```bash
$ mkdir ~/.ethereum ~/.ethash ~/.web3
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

If running in detached mode, you can view the logs with the following command:
```bash
$ docker logs -t ethminer-cpp
```

## Bash Script

You can optionally also use the provided `bin/docker-ethminer` bash script to
start/stop the service on a schedule.
Edit the volume mounts and mining parameters as needed before running the script.

## Docker Stack

You can optionally also use the provided `docker-compose.yml` for running as
a service either with docker-compose or docker swarm.
Edit the volume mounts and entrypoint as needed before creating stack.

### Create Stack

Create a new stack with the
```bash
$ docker stack deploy --compose-file docker-compose.yml miner
```

### Update Stack

The same deploy command will pull the latest image and update the container as needed.
```bash
$ docker stack deploy --compose-file docker-compose.yml miner
```

### Remove Stack

This will stop the miner and remove the stack.
```bash
$ docker stack rm miner
```

### Stop Mining

This will stop the ethminer-cpp service.
```bash
$ docker service scale miner_ethminer-cpp=0
```

### Start Mining

This will start the ethminer-cpp service.
```bash
$ docker service scale miner_ethminer-cpp=1
```

## Sources

* https://github.com/ethereum/cpp-ethereum
* https://github.com/ethereum/cpp-ethereum/tree/develop/scripts/docker

## Documentation

* http://www.ethdocs.org/en/latest/ethereum-clients/cpp-ethereum/index.html
* http://www.ethdocs.org/en/latest/ethereum-clients/cpp-ethereum/building-from-source/linux.html
* http://www.ethdocs.org/en/latest/ethereum-clients/cpp-ethereum/installing-binaries/docker.html
