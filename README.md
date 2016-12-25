# vagrant-docker-swarm-mode

This project sets up a [Docker Swarm Mode][1] cluster. It will create X managers (defaults to 1) and Y workers (defaults to 1).

## Usage
Just start your cluster with ```vagrant up``` to kick in with the default setup. To modify the number of managers and workers edit the Vagrant file and modify the appropriate constants in the beginning of the file:
```
NUM_OF_MANAGERS=1
NUM_OF_WORKERS=1
```

### Adding Managers and Workers
If additional managers and/or workers need to be added to an existing swarm just update the same variables as shown above and run ```vagrant up``` again. The new nodes will be added to the swarm.

### Cleanup
To start from scratch make sure to remove the 'swarm-token' folder which will be created by the first manager started. Something like the following would make sense:
```
vagrant destroy --force
rm -rf swarm-token
```

This folder is used to store the tokens (managers and workers) for joining the swarm.

[1]: https://docs.docker.com/engine/swarm/
