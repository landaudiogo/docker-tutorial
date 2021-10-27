# Overview

Learning container networking.

Since container is isolated from the host, we have to expose the ports the container has to listen in to allow it to communicate with the external world through the host.

As mentioned in the README.md documentation, the EXPOSE instruction is more of a documentation for the developer to know how the container is supposed to communicate.

The goal is to learn:
- accessing the container from the host
- communicate between 2 containers within the same host
- using an external network communicate between containers.

## Host communicating with container

```
docker build -t t3i .
docker run -it --rm -p 80:1234 --name t3c t3i
```

From a browser or using the wsl terminal, try to connect to localhost:80.

## Connecting 2 containers

It might be the case that we want 2 containers to communicate with one another. For this, they require to be on the same network.

There are 2 ways to do this.

## External network to connect 2 containers

