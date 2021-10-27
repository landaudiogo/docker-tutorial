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

We will now create the server and client images:
```
docker build -t t3i-server -f Dockerfile-server .
docker build -t t3i-client-1 -f Dockerfile-client-1 .
```

We start our server:
```
docker run -it -p 80:1234 --rm --name t3c-server t3i-server
```

When we try to run our client, we know that we have our container listening on port 1234. When we reference localhost within the container, confusingly, we are not referencing the actual host, it is the ip address of the actual container. So we cannot use `localhost:80` either to connect via the host.

We also mapped the host's port 80 to the container's port 1234, so if we try to communicate with our host through port 80, it is forwarded to the container's 1234 port.

```
docker run -it --rm --name t3c-client-1 t3i-client-1
```

This won't work because the container does not have an alias for the host "t3c-server".

We also cannot use localhost as that represents the actual container that is running.

The only alternative is specifying the actual host ip by visiting the ifconfig within the container.

```
docker build -t t3i-client-2 -f Dockerfile-client-2 .
docker run -it --rm --name t3c-client-2 t3i-client-2
```

But it is very host dependent, as the actual ip address of the container would have to be obtained, as performed.

We will now do the same, but with the first build which uses an alias for the container's ip address.
To do so, the only thing we have to change, is add both containers to the same network. For this we do:
```
docker network create t3n

# on the first terminal
docker run `
    -it -p 80:1234 `
    --rm --name t3c-server `
    --network t3n `
    t3i-server

# on the second terminal
docker run `
    -it `
    --rm --name t3c-client-1 `
    --network t3n `
    t3i-client-1
```

Now we clean up:
```
docker network rm t3n
docker image rm t3i t3i-server t3i-client-2 t3i-client-1
```


