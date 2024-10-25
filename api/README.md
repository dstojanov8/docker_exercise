# Docker Image Overview

Every (almost) line we write inside the Dockerfile represents a new layer in the image we are creating. First layer we pull in the parent image and so on... This is how the images are being build. Layers being stacked on top of each other. Images are read only, they dont automatically update after changes. If we change something in our code we need to create a new image.

## Docker commands

First Navigate to api directory (direcroty where Dockerfile is located)

`docker build -t <image_name> .` - run to create a new image, -t to tag (give the image a name)

- we use . as a relative path to the Dockerfile from our directory

`docker build -t <image_name>:<tag_name> .` - we can specify tag name (version) of the image (this is optional)

- we do need to use both (image_name:tag_name) when creating a new container with this image

`docker images` - lists all docker images (here we get the name (repository) and the image id)

`docker run --name <container_name> -p <comp_port>:<cont_port> -d --rm <image_name/image_id>`

- runs an image and creates + STARTS the container, we specify the container name `--name` that is <container_name>
- the image name/id which we are using that is <image_name/image_id>
- we specify the port `-p` <comp_port> is port on out computer that is mapped to <cont_port> port exposed by the container
- use `-d` to detach the terminal from the container process (we will be able to use the terminal)
- use `-rm` to remove/delete the container once we stop it later on

`docker ps` - shows us a list of running containers

`docker ps -a` - shows us a list of all docker containers

`docker start <container_name/container_id>` - just STARTS the container, without creating a new one

- no port configuration needed or `-d` if it is specified during the container creation process

`docker stop <container_name/container_id>` - stops the container with the specified name/id

`docker image rm <image_name/image_id>` - delete an image with specified name/id

- will only work if the image is not used by any container

`docker image rm <image_name/image_id> -f` - will force delete an image with specified name/id

- we can also first delete the container that uses the image and then the image itself

`docker container rm <container_name/container id>` - delete container with name/id

`docker system prune -a` - delete all images, containers and volumes

## Docker Volumes

Volumes are a feature in **Docker** that allow us to specify folders on our host computer to be made available to running containers. We can map these host folders to specific folders inside the container so that if something changes in these folders on our computer, the change is also reflected in the mapped folders in the container.

During development, we can use volumes to map our project folder to a container volume. This way, any change in our source code is immediately reflected in the container. However, if we have a `node_modules` folder inside our app folder and delete it, that deletion will also be reflected in the container. **Solution**: add an anonymous volume.

**Anonymous volumes** map a container’s folder (in this case, `node_modules`) to a temporary location on the host machine (managed by Docker). In the example below, `/app/node_modules` specifies the folder location inside the container when it’s running. We don’t map it to any specific folder on our computer; Docker handles this automatically. The contents of this folder will **persist** even after the container stops, so when it starts again, those files will still be available.

ex.
`docker run --name myapp_c_nodemon -p 3000:4000 --rm -v "C:\Users\dimitrije\Desktop\Docker Course\api:/app" -v /app/node_modules myapp:nodemon`

`-v "C:\Users\dimitrije\Desktop\Docker Course\api:/app"` -> volume to map out project folder

`-v /app/node_modules` -> anonymous volume for node_modules

`docker run --name <c_name> -p <comp_port>:<cont_port> -d --rm -v <project_path>:<container_path> <i_name/i_id>`

- `<project_path>` is an aboslute path to our project (right click on the project and get path)

- `<container_path>` is where we want to map this folder to in the container

## Layer Caching

Every time docker tries to build an image and walks through different layers, its stores each layer of our (new) image in cache. The first time we build the image, after each layer, docker took our (new) image in that point in time and stored in the cache. When we build the image again, before docker starts the build process from scratch, it looks in our cache and tries to find the image in our cache that it can use for the new image we are creating. Once Docker finds a layer that has chaged it no loner uses cached information. A good way to optimize this is to add more individual COPY layers so we can cache RUN commands as well.

### Additional pointers

1. Check Dockerfile for explanations of eact instruction

2. It is best not tu run `npm install` on local machine
   (COPY . .) command will, in that case, copy node_modules folder as well
   (RUN ...) command will replace the existing
   This can cause problems with package versions and COPY command will take more time

   If we do have a node_modules folder on local machine than put it in `.dockerignore`
