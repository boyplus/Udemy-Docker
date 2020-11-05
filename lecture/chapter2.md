# Udemy Docker Course

## Chapter 2 Manipulating Containers with Docker Client

#### Create and run Containers from image

```dockerfile
docker run <image name>
```

- docker -> Reference the Docker Client
- run -> Try to create and run a container
- image name -> Name of image to use for this container

```dockerfile
docker run hello-world
```

Image hello-world, the FS Snapshot is hello-world and startup command is "Run hello-world".

```dockerfile
docker run <image name> <command>
```

- command -> default command override

```dockerfile
docker run busybox ls
```

> The default command was override to be **ls** (we don't know the default command) so it will list all directory in busybox snapshot which is bin, dev, etc, home, proc, root.

---

#### List all running containers

```dockerfile
docker ps
docker ps --all
```

- **ps** is to list all runing containers
- **-all** is to list all containers (create + start + shutdown)

```dockerfile
docker run => docker create + docker start
```

---

#### Create a container

> Setting up the FS Snapshot inside the container. After we create container, we will get the id of the container

```dockerfile
docker create <image name>
docker create hello-world
```

---

#### Start a Container

> Actually start the container (run the command)

```dockerfile
docker start <container id>
docker create hello-world
docker start 7bc37f3d74bd733918377
docker start -a 7bc37f3d74bd733918377
```

> We use attribute -a to tell that we want to see the output after we start the container, if we did not use -a, the containers just run but we did not see any output of them.

---

#### Example of busybox Image (override command)

```dockerfile
docker run busybox echo hi there
docker run busybox ping google.com
```

> If we try to override the default comand of hello-world image such as echo, ping. It will get an error because FS Snapshot of hello-world has only hello-world so we cannot use other command (this image did not provide other commands)

---

#### Docker Life Cycle

-> After we create and start a container, please note that the container is just stop (Exited) but the container is not killed yet. That means we can run the container again

```dockerfile
docker run busybox echo hello-busybox
docker start -a 7bc37f3d74bd733918377
docker start -a 7bc37f3d74bd733918377 echo hi there //we cannot do like this
```

> Also please note that after we create a container, when we run a container, we cannot override the command.

---

#### System prune

```dockerfile
docker system prune
```

In this command, it will deleted all of these

- all stopped containers
- all networks not used by at least one container
- all dangling umages
- all build cache

-> That means we have to wait a few second when we download an image from docker hub.

---

#### Get logs from a container

```dockerfile
docker logs <container id>
```

#### Stop a container

```dockerfile
docker stop <container id>
```

#### Kill a container

```dockerfile
docker kill <container id>
```

#### Execute an additional command in a container

```dockerfile
docker exec -it <container id> <command>
```

- **exec** -> Run another command
- **-it** -> Allow us to provide input to the container
- **command** -> Command to execute

For example, 

