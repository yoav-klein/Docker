
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
