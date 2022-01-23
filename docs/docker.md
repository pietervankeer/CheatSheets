# Docker

| Task                                          | Command                               |
| :-------------------------------------------- | :------------------------------------ |
| Hello world!                                  | `docker run hello-world`              |
| List running containers                       | `docker ps`                           |
| List all containers (running and not running) | `docker ps -a`                        |
| List docker images                            | `docker images`                       |
| Remove docker container                       | `docker rmi hello-world`              |
| Remove docker container                       | `docker rm ID`                        |
| Run commands on detached container            | `docker exec -t alpine /bin/hostname` |

## Docker RUN options

| Task                    | Option                    |
| :---------------------- | :------------------------ |
| Tag container           | `--name container:latest` |
| Interactively           | `-i`                      |
| Open a shell (terminal) | `-t`                      |
| Detached mode           | `-d`                      |

## Persistent data

| Task           | Command                            |
| :------------- | :--------------------------------- |
| Create volume  | `docker volume create mysql-data`  |
| Inspect volume | `docker volume inspect mysql-data` |

When creating a container you can assign the volume.

> `-v mysql-data:/var/lib/mysql`

## Custom images

| Task                  | Command                              |
| :-------------------- | :----------------------------------- |
| Create image          | `docker image build Dockerfile`      |
| More info about image | `docker image inspect alpine:latest` |
