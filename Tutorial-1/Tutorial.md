# Overview

Goals:
- Learning what a container and an image are.
- Creating an image.
- Deploying a container.
- Learning how to interact with the container.
- What is the ENTRYPOINT instruction.
- What is the CMD instruction.
- Why the exec format is important.

First, run the following lines:
```
docker build -t t1i-exec -f Dockerfile-exec .
docker build -t t1i-shell -f Dockerfile-shell .
```
The above line creates an image with the tag "t1i" which is the name given to the image, short for tutorial-1-image, and a second name either (shell|exec), to represent the form in which the ENTRYPOINT is written.

We are first going to see how the exec form performs.

We first create a container, with one of these options (first one is preferred):
1. create a container with a command, then start the container with another
```
docker container create -t -i --name t1c --rm t1i-exec
docker start -ai t1c
```

2. create a container and run it in a single command
```
docker run -i -t --name t1c --rm t1i-exec
```

On another terminal, we will now send a termination signal to the container, so we can see what happens when we try to catch the signal:
```
docker stop t1c
```

since we have a signal handler in the python code, we are capable of catching the signal and performing any last operations before the container terminates. It aslo increases the speed with which the process stops as will be seen in the next example.

Now we review the shell form ENTRYPOINT, and it's differences from the exec form:
1. create the container
```
docker run -it --rm --name t1c t1i-shell
```
2. send the stop signal from another terminal:
```
docker stop t1c
```

Another difference of using the shell and exec form of ENTRYPOINT, is that the container does not pass the CMD arguments, or the runtime arguments if ENTRYOPINT is writter in shell form.

Now we clean up. Because the container's ran with the --rm flag, they are removed from docker as soon as they stop, otherwise, they are persisted.
The only resource we have to clean are the images we created:
```
docker image rm t1i-exec t1i-shell
```