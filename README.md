# Docker

## Install docker desktop

Windows:

https://www.docker.com/products/docker-desktop 

Mac :

https://hub.docker.com/editions/community/docker-ce-desktop-mac?utm_source=docker&utm_medium=webreferral&utm_campaign=dd-smartbutton&utm_location=header


CHOCOLATEY:

1 . Install Chocolatey -  https://chocolatey.org/install
2 . Install Docker - choco install docker-desktop



## Manage Containers

| Command                      | Description                               |
|------------------------------|-------------------------------------------|
| `docker run [-d] image_name` | Run (and download) a new docker container |
| `docker ps`                  | List running containers                   |
| `docker ps -a`               | List all containers                       |
| `docker stop container_id`   | Stop container                            |
| `docker start container_id`  | Start and stopped container               |
| `docker rm container_id`     | Remove container (Docker has a GC)        |


## Work with a containers
| Command                            | Description                    |
|------------------------------------|--------------------------------|
| `docker exec container_id command` | Execute a command              |
| `docker exec -it container_id sh`  | To open a shell                |
| `docker logs container_id`         | Show the output of a container |

## Image Management
| Command                                      | Description                                               |
|----------------------------------------------|-----------------------------------------------------------|
| `docker pull image_name`                     | Download an image                                         |
| `docker images`                              | Show images                                               |
| `docker rmi image_id`                        | Remove image                                              |
| `docker build -t image_name dockerfile_path` | Build an image from a Dockerfile (See Dockerfile section) |

## Comunication with the container
| Command                                                 | Description                                                          |
|---------------------------------------------------------|----------------------------------------------------------------------|
| `docker run -p [src_port]:[dst_port] image_name`        | Publish a container's port to the host                               |
| `docker run -dp [src_port]:[dst_port] image_name`       | Publish a container's port to the host in detached mode (background) |
| `docker run -v [host_path]:[container_path] image_name` | Mount host path                                                      |

## Comunication between containers (networks)

Docker by default assigns all the containers to a network named bridge. With the following commands you can create a network and connect/disconnect the containers.

| Command                                                                  | Description                                                      |
|--------------------------------------------------------------------------|------------------------------------------------------------------|
| `docker network ls`                                                      | List networks                                                    |
| `docker network create network_name`                                     | Create a new network                                             |
| `docker network connect [--alias host_name] network_name container_id`   | Connect a container to a network                                 |
| `docker network disconnect network_name container_id`                    | Disconnect a container from a network                            |
| `docker network inspect network_name`                                    | Show the network configuration                                   |
| `docker run --network network_name --network-alias host_name image_name` | Run a container and connect it to a network with a specific name |


## Persistence (volumes)

| Command                                            | Description                        |
|----------------------------------------------------|------------------------------------|
| `docker volume ls`                                 | List volumes                       |
| `docker volume create`                             | Create a volume                    |
| `docker run -v volume_name:mount_point image_name` | Run a container and mount a volume |

# Docker compose

To simplify the use of docker, we can create a yaml file to define all these parameters (named docker-compose.yml). 

```yml
version: "3.9"
services:
  webserver1:
    image: httpd:latest
    ports:
      - 80:80
    volumes:
      - ./:/usr/local/apache2/htdocs/
  webserver2:
    image: httpd:latest
    ports:
      - 81:80
    volumes:
      - www:/usr/local/apache2/htdocs/
  webserver3:
    image: httpd:latest
    ports:
      - 82:80
volumes:
  www:
```

Run it with the following command:
```bash
docker-compose up -d
```

Docker compose automatically creates a network with all containers. Each container is assigned a network name to be able to communicate with each other without depending on the IP.
