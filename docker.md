# Docker

Components:
- Image
- Container: container to docker is like instance to class.
- repository: a place to store the images, e.g. Docker Hub

```bash
# check docker version
docker version

# check all downloaded images
docker images

# check all of the containers 
docker container ls 

# check running containers
docker ps

# delete temporary images -a: delete all
docker image prune 

# search images of mysql
docker search mysql

# pull the images
docker pull mysql:5.7

# Delete an image 
docker stop [container]
docker rm [container name/id]
docker rmi [image]

# Use the docker 
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
# e.g. -p stands for publish, -d stands for detach, which means running in the background
docker run -p 8000:8080 -d --name myname mysql

```

