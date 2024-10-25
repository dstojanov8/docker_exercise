# Project Overview

Small exercise to see how **_Docker_** and **_docker-compose_** works.

- In the _/api_ directory there is a simple backend app made with Node's **Express** framework. This app exposes an **API** on `port 4000`, a **GET ('/')** request, returning a list of objects with an `id` and a `title`.
- In the _/myblog_ directory, there is a simple frontend **React** app, which fetches and displays the objects received from the backend. The React app runs on `port 3000`.

In **Dockerfiles** that are placed inside the two app directories, _ports 4000_ and _3000_ are **exposed** respectively, as these are the ports each app uses inside its container. The **docker-compose.yml** file maps these internal container ports to ports accessible outside the container.

- In the case of our backend, we are mapping the `port 4000` (internal) to `3000` (external). Since the container that will be runnung our frontend app will make a request to the backend we are sending that request to `port 3000`, as it is the one that will be used outside the container.
- In the case of our frontend, we are mapping the `port 3000` (internal) to `2000` (external), and to access the app in our browser will will need to go to `http://localhost:2000/`, as `port 2000` will be used outside the container.

_For example and information on docker-compose check the `.yml` file_
**For more information about Dockerfile and now Docker works please go to /api/README.md and Dockerfile-v1 and Dockerfile**

# Docker Compose Overview

Builds (makes) images and runs them to create containers based on the confuguration provided in docker-compose.yml file.

`docker-compose up` - run docker-compose file to start start the container

`docker-compose down` - stops the container and delets it, image and volumes will remain

- if we add `--rmi all -v` flags in this command image and volumes will be removed as well

## Check docker-compose.yml file. Explanation of volumes

```
volumes:
  - ./api:/app
  - /app/node_modules
```

1. `./api:/app`: This maps the `./api` directory from your host machine to `/app` inside the container, making any changes in your local `api` folder visible inside the container.

2. `/app/node_modules`: This creates an anonymous volume in Docker, which means:

- Docker will store the data for `/app/node_modules` outside the container's writable layer.
- This anonymous volume does not map to any specific folder on your host (unlike ./api:/app).
- Docker uses this to ensure that any files in `/app/node_modules` inside the container are isolated and not overwritten by the host.
