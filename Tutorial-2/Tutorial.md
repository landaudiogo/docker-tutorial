# Overview

The goal of this tutorial is to understand docker's volume.

Some common use cases for volumes include:
- sharing data between containers
- persisting data between container runtimes

As mentioned in the README.md file in the root directory of this repo, the VOLUME instruction indicates where there are going to be any mount points in the container's filesystem.

The Dockerfile indicates which directory is going to have a volume mounted to it.

On runtime, user specifies which host or docker volume attach to that directory with the:
```
-v or --volume
docker run ... --volume example_docker_volume:<container_path>
docker run ... -v example_docker_volume:<container_path>
```

Before we perform the tests, we first create the image with which we will start the container:
```
docker build -t t2i .
```

## Creating a docker volume

Run the following command:
```
docker run `
    -it --rm `
    --name t2c-1 `
    --volume t2v:/usr/src/data `
    t2i
```

```
docker run `
    -it --rm `
    --name t2c-2 `
    --volume t2v:/usr/src/data `
    t2i `
    /bin/sh -c 'echo "sharing" >> ../data/save_file'
```

To check the docker volumes we have created:
```
docker volume ls
```

### Using docker-compose

```
docker-compose build
docker-compose up
```

## Creating a host volume

```
docker run `
    -it --rm `
    --name t2c-1 `
    --volume ${PWD}\data:/usr/src/data `
    t2i
```

```
docker run `
    -it --rm `
    --name t2c-2 `
    --volume ${PWD}\data:/usr/src/data `
    t2i `
    /bin/sh -c 'echo "hello" >> ../data/save_file'
```



## Creating a host docker volume

This creates the volume and specifies the host path to mount
```
docker volume create --opt type=none --opt device=${PWD}/test --opt o=bind t2v-2
```

Now we create the 2 containers that will access this volume, as if it were a docker container.

```
docker run `
    -it --rm `
    --name t2c-1 `
    --volume t2v-2:/usr/src/data `
    t2i
```

```
docker run `
    -it --rm `
    --name t2c-2 `
    --volume t2v-2:/usr/src/data `
    t2i `
    /bin/sh -c 'echo "hello" >> ../data/save_file'
```

Now we clean up.
First we delete the new data folder we created in this directory.
```
rd -r data
rd -r test
docker volume rm t2v
docker-compose rm
docker image rm tutorial-2_t2c-2 tutorial-2_t2c-1 t2i
```