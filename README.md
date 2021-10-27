# Overview

> Official docs:
> https://docs.docker.com/get-started/

All the concepts presented in this document are a summary of what is presented in the documentation above.

## Dockerfile Instructions

> Check the documentation for more information
> https://docs.docker.com/engine/reference/builder/#from

### FROM

represents the base image to be used by the following instructions in the Dockerfile.

After a FROM instruction, what comes after it is called a build stage. Check the documentation for multiple build stages.

### RUN 

execute any command in a new layer, and commit the new layer on top of the current image. The resulting image is then used for the following instructions in the dockerfile.

### CMD

The main purpose of a CMD is to provide defaults for an executing container. There can only be one CMD instruction, and if there are more in a dockerfile, then the last is the one that takes effect.

### EXPOSE

The EXPOSE instruction informs Docker that the container listens on the specified network ports at runtime. You can specify whether the port listens on TCP or UDP, and the default is TCP if the protocol is not specified.

The EXPOSE instruction does not actually publish the port. It functions as a type of documentation between the person who builds the image and the person who runs the container, about which ports are intended to be published. To actually publish the port when running the container, use the -p flag on docker run to publish and map one or more ports, or the -P flag to publish all exposed ports and map them to high-order ports.

### ENV

The ENV instruction sets the environment variable <key> to the value <value>. This value will be in the environment for all subsequent instructions in the build stage and can be replaced inline in many as well. The value will be interpreted for other environment variables, so quote characters will be removed if they are not escaped. Like command line parsing, quotes and backslashes can be used to include spaces within values.

### COPY 

The COPY instruction copies new files or directories from <src> and adds them to the filesystem of the container at the path <dest>.

### ENTRYPOINT

An ENTRYPOINT allows you to configure a container that will run as an executable.

Command line arguments to docker run <image> will be appended after all elements in an exec form ENTRYPOINT, and will override all elements specified using CMD. This allows arguments to be passed to the entry point, i.e., docker run <image> -d will pass the -d argument to the entry point. You can override the ENTRYPOINT instruction using the docker run --entrypoint flag.

The shell form prevents any CMD or run command line arguments from being used, but has the disadvantage that your ENTRYPOINT will be started as a subcommand of /bin/sh -c, which does not pass signals. This means that the executable will not be the container’s PID 1 - and will not receive Unix signals - so your executable will not receive a SIGTERM from docker stop <container>.

There is a way to do it, which would be by running it as with `exec` as the first command of ENTRYPOINT in the shell form.

Only the last ENTRYPOINT instruction in the Dockerfile will have an effect.

### VOLUME

> Volume docs:
> https://docs.docker.com/storage/volumes/

The VOLUME instruction creates a mount point with the specified name and marks it as holding externally mounted volumes from native host or other containers.

The docker run command initializes the newly created volume with any data that exists at the specified location within the base image. Refer to documentation for an example of existing data.

**The host directory is declared on runtime.**

### ARG

The ARG instruction defines a variable that users can pass at build-time to the builder with the docker build command using the --build-arg <varname>=<value> flag.

An ARG instruction can optionally include a default value


# Tutorial 1 

Review of what a container and an image are, and applying the concepts to deploy a "hello-world" python application.

## Container

A container is a sandboxed process on a machine that is isolated from all other processes on the host machine.

## Image (or Container Image)

When running a container it uses an isolated filesystem. This custom filesystem is provided by an image. 

Since the image contains the containers filesystem, it must also contain all it's dependencies to successfully run the application.

The common way to set up an image has an image being built on top of another existing image.

In our case, this first tutorial will use the Python base image, add it another files to it's custom filesystem through the copy command, and then indicate for it to run the command:

```python3 main.py```

There are several types of commands that can be included in the Dockerfile, this link describes the list of commands that can be executed within the Dockerfile.
