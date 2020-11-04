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
docker run <image name> command
```

- command -> default command override



```dockerfile
docker run busybox ls
```

-> The default command was override to be **ls** (there is no default command) so it will list all directory in busybox snapshot which is bin, dev, etc, home, proc, root.



