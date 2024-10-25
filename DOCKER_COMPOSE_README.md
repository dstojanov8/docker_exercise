# Docker Compose OVERVIEW

Builds (makes) images and runs them to create containers based on the confuguration provided in docker-compose.yml file.

`docker-compose up` -> run docker-compose file to start start the container

`docker-compose down` -> stops the container and delets it, image and volumes will remain
-> if we add `--rmi all -v` flags in this command image and volumes will be removed as well

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
