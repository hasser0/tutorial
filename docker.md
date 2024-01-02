
- [Containers](#containers)
- [Images](#images)
  - [Pushing images](#pushing-images)
  - [General structure of images](#general-structure-of-images)
- [Networks, Volumes and Mounts](#networks-volumes-and-mounts)
- [Dockerfile](#dockerfile)
  - [CMD vs ENTRYPOINT](#cmd-vs-entrypoint)

# Containers
```sh
docker container ls [--all]
docker container run
    [--name <container-name>]
    [--interactive]
    [--detach]
    [--tty]
    [--volume <volume-name>:<path>]
    [--publish <host_port>:<container_port>]
    <image-id>
docker container rm [--force] <container-id>
docker container stop <container-id>
docker container start [--interactive] <container-id>
docker container top <container-id>
docker container stats <container-id>
docker container inspect <container-id>
docker container logs <container-id>
# container_file = <container-name>:<path>
docker container cp < local_file | container_file > < container_file | local_file >
docker exec --interactive --tty <container-id> <command>
```

# Images
```sh
docker image ls
docker image pull <image-id>
docker image build [--tag image_name:tagt]
docker image rm <image-id>
```
## Pushing images
```sh
docker login --username <username>
docker commit <container-id> <new-image-name>
docker image tag <image-id> <username>/<image-name>:<version-tag>
```

## General structure of images
```sh
export REGISTRY_DOMAIN = mcr.microsoft.com
export OWNER_ACCOUNT = mssql
export IMAGE_NAME = server
export IMAGE_TAG = 2022-latest
echo $REGISTRY_DOMAIN/$OWNER_ACCOUNT/$IMAGE_NAME:IMAGE_TAG
> mcr.microsoft.com/mssql/server:2022-latest
```

# Networks, Volumes and Mounts
Networks can be used to connect several containers
```sh
docker network ls
docker network create <network-name>
docker network rm
docker network connect <network-name> <container-id>
```

Volumes are persistent storage
```sh
docker volume ls
docker volume create <volume-name>
docker volume rm <volume-name>
```

Mounts and volumes are different, a volume can be accessed only from containers. On the other hand mounts can be accessed from containers and hosts.
```sh
docker run --mount type=bind,source=<source-path>,target=<target-path> <image-id>
docker run --volume <source-path>:<target-path> <image-id>
```

# Dockerfile
```Dockerfile
FROM diamol/node AS build-stage
RUN echo 'Running on build-stage while building image' > /logs.txt
ENV TARGET="google.com"
WORKDIR /web-ping
VOLUME /data
COPY app.js

FROM diamol/node AS test-stage
COPY --from=build-stage /logs.txt /logs.txt
RUN echo 'Running on test-stage while building image' >> /logs.txt
CMD ["echo", "'Running while container is running'"]
CMD ["node", "/web-ping/app.js"]
EXPOSE 5000
```
- `FROM` starts from another image / multi-stage images
- `RUN` run commands during build
- `ENV` set environment variables
- `WORDIR` `mkdir` and `cd`
- `VOLUME` create a persistent volume
- `COPY` copy files from local to the container
- `CMD` run commands when container starts and can be overwritten with `-it <container-id> <new-command>`
- `ENTRYPOINT` to make a docker file like an executable, cannot be overwritten
- `EXPOSE` expose a port to the host

## CMD vs ENTRYPOINT
CMD are used to create daemons and services
```Dockerfile
FROM python:3.8-alpine
RUN echo 'print("hello world!")' > /script.py
CMD ["python3", "script.py"]
```
then build `docker build -t python-cmd .`
and then run `docker run python-cmd` vs `docker run -it python-cmd bash`

ENTRYPOINT are used to create "executable" containers
```Dockerfile
FROM python:3.8-alpine
ENTRYPOINT ["python3"]
```
then build `docker build -t python-entrypoint .`
and then run `docker run python-entrypoint script.py` vs `docker run -it python-entrypoint bash`
