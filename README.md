# vagrant-docker-swarm-mode

This project sets up a [Docker Swarm Mode][1] cluster. It will create X managers (defaults to 1) and Y workers (defaults to 1).

## Usage
Just start your cluster with ```vagrant up``` to kick in with the default setup. To modify the number of managers and workers edit the Vagrant file and modify the appropiate constants in the begining of the file:
```
NUM_OF_MANAGERS=1
NUM_OF_WORKERS=1
```

[1]: https://docs.docker.com/engine/swarm/
