There are times when we want have a http request to external APIs or websites via. Or we might even want to communicate with a service in our host machine as well (e.g. database maybe). There are also scenarios where we want our container to talk to another container.
_Sending HTTP requests from inside a container to the world-wide-web JUST works without any configurations or settings_.

Containers normally can't talk to each other, unless you create a container network or lookup container's IP address.

## Creating Container Networks

The idea is that when having multiple containers and wanting to allow communication between these containers. With Docker, you can put all these containers into one network with `--network <NETWORK-NAME>`. Keep in mind that you have to create the network on your own first `network create <NETWORK-NAME>`:
```bash
# Create a network with name "favorites-net"
$ docker network create favorites-net

# run the container with the same network name
$ docker run ... --network favorites-net ...
```
Within a Docker network, all containers can communicate with each other and IPs are automatically resolved.
All containers to share the network, make sure to run those containers with the above flag.

Keep in mind, when having container to container connection within same network, there's no need to publish the port (`-p`), because all the containers can freely communicate to each other.


