
# Docker Deep Dive


## Creating Containers

Run container with `docker run IMAGE`

Flags:
`--rm` - remove the container when it exits.
`-d` - run in detached mode (background)
`-i` - keep stdin open even if not attached to the container
`-t` - allocate a pseudo-tty for the container
`-p` - publish container port by mapping it to a port in the host. 
`-P` - publish all exposed ports to the host interface
`-v` - mount a volume.


#### To Learn
- What does `-i` and `-t` actually do? What is TTY and all that..


## Exposing Ports

When we run for example the `nginx` container like this: <br>
`docker run -d nginx`
We are not exposing the port is listens on, so it's not accessible. <br>
If we try to <br>
`curl localhost`
We get <br>
```
curl: (7) Failed to connect to localhost port 80: Connection refused
```

It is accessible though in it's own internal IP, which we can see if we: <br>
```
$> docker container inspect <CONTAINER_ID>

 "IPAddress": "172.17.0.3",
```

To deal with that, we need to map the port that the contained application is listening on, <br>
to a specific port on the host: <br>

```
$> docker run -p 8080:80 nginx
```

will map port 80 in the container to port 8080 in the host.

## Execute Conatiner Commands

How to execute commands on a container. 

Few ways:

First way is telling the image what command to execute, by the Dockerfile. <br>
Second way - execute docker run and define what command we want to execute by the container. <br>
Third way - executing `docker conatiner exec`. There are few things to know about this command: <br>
First, it'll only run when the container's primary process is running (?) <br>
also, it'll execute in the default directory of the conatiner (?)

You can also run `exec /bin/bash`, which will take you into the container and then you can <br>
execute which command you like. <br>

The command that's being executed when the container runs may be a "One and done" <br>
which makes the container a task, or it's gonna be a long running process. <br>

Let's try each of these out: <br>
We're going to run a `Nginx` container:
```
  $> docker run -d nginx
```

Now you can see (at least at some dockerfile of Nginx), that the COMMAND is <br>
```
CMD ["nginx", "-g", "daemon off;"]
```
This is how we define what command will be executed once the container runs. <br>

Now, let's try to run nginx in the following way: <br>
```
  $> docker run -it nginx /bin/bash
```
Now, the Nginx server wasn't started, since we overriden the CMD directive with our own! <br>
Now, go and run nginx from within the container with the command in the Dockerfile:
```
  $> nginx -g 'daemon off;'
```
Now you're blocked. Now try to curl that IP and see that you get a reply. <br>




