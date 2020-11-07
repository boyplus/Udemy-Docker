# Udemy Docker Course

## Chapter 3 Building Custom Images Through Docker Server

#### Creating Docker Images

In order to create an docker image, we have to write the docker file (text file) and the flow is like this

- Dockerfile -> Configuration to define how our container should behave
- Docker Client
- Docker Server
- Usable Image

**Creating a Dockerfile**

- Specify a base image
- Run some commands to install additional programs
- Specify a command to tun on container startup

----

#### Create an image that runs redis-server

```dockerfile
# Dockerfile
# Use an existing image as a base
FROM alpine

# Download and install dependency
RUN apk add --update redis

# Tell the image what to do when it starts as a container
CMD ["redis-server"]
```

```dockerfile
docker build .
docker run 25cf8724863e
```

The Command compose of two things

- FROM, RUN, CMD -> Instruction telling Docker Server what to do.
- Argument to the instruction

Writing a dockerfile == Being given a computer with no OS and being to install Chrome

**Flow of install Chrom in computer with no OS**

- Install an Operating System (Specify a base image)
- Start up your default browser (Run command to install additional programs)
- Navigate to chrome.google.com
- Download Installer
- Open file/floder explorer
- Execute chrome_installer.exe
- Execute chrome.exe (Command to run on startup)

---

**Why did we use alpine as a base image ?**

-> Why do you use Windows, MacOS, Ubuntu ?

-> They come with a preinstalled set of programs that are useful to you!

-> We use alpine because it has the package manager (apk) for us to install additional programs

---

#### The build process in detail

```dockerfile
docker build .
```

- build -> command to build the Dockerfile
- . -> build context

**The three steps behind the scene**

- Step 1/3 FROM alpine

> check whether docker cache has alpine image or not, if yes they will use that cache, otherwise it will download from docker hub repository

- Step 2/3 RUN apk add --update redis

> we can see that docker server create the addtional container and remove it for us. (Temporary Container). That means, when we specify the base image, docker will create a container of alpine image and run the command as a running process to install additional programs (redis), so we get additional file in hard drive. After it remove that container, docker will create an new image for us that already installed redis inside the image.

- Step 3/3 CMD ["redis-server"]

>  look to the previous image, create the container, and set the primary command. After that it remove the temporary container and create an new image that already has primary command when container start for us.

**Recap the flow of build image**

- Download alpine image (base image)
- **RUN apk add --update redis**
- Get image from last step (first step)
- Create a container out of it -> Container!
- Run 'apk add --update redis' in container -> Container with modified FS
- Take snapshot of that contains's FS -> FS Snapshot
- Shut down that temporary container
- Get image ready for next instruction
- **CMD ["redis server"]**
- Get image from last step (second step)
- Create a container out of it -> Container!
- Tell container it should run 'redis-server' when started -> Container with modified primary command
- Shut down that temporary container
- Get image ready for next instruction

Output is the image generated from previous step.

---

#### Rebuild with cache

```dockerfile
# Dockerfile
# Use an existing image as a base
FROM alpine

# Download and install dependencies
RUN apk add --update redis
# Add more dependencies
RUN apk add --update gcc

# Tell the image what to do when it starts as a container
CMD ["redis-server"]
```

-> when we already install image (base image), dependencies (RUN apk add --update redis), docker will use the cache for us. That means, we can build image faster.

Please note that the order of installing dependencies is matters, that means if we install gcc before install redis, docker will not use cache anymore, but it will install gcc and redis respectively.

---

#### Tagging and image

```dockerfile
docker build -t boy78111947/redis:latest .
```

- boy78111947 -> your docker ID
- /redis -> repo/project name
- latest -> version of image (can be number)
- . -> build context (do not forget the build context)

```dockerfile
docker run boy78111947/redis:latest
docker run boy78111947/redis
```

> If we did not specify the version of image, docker will use the latest of that image.

---

#### Docker Image generation with docker commit

-> In facts, we can create the image from the container like docker server do when we build the dockerfile behind the scene for us.

-> We did not do somethings like this often just let you to know that we can do like this.

```dockerfile
docker run -it alpine sh //create a container with alpine as a base image
apk add --update redis //install redis inside that container

# Open another terminal and get ID of the above container (docker ps)
docker ps
docker commit -c 'CMD ["redis-srever"]' 23d50e30b188 //set up the primary commadn when container start, id is the previous container id

# We will get the id of new image with base image of alpine and dependencie redis and start command 'redis-server' inside that image
docker run 8574fcd5de418fbe44 //if from last step
```



---



