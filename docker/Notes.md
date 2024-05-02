```bash
# remove dangling resources (not tagged or associated with a container)
docker system prune
# remove any stopped containers, all unused images
docker system prune -a

docker build -t <name> .

docker run <name>
docker run -it <name> bash
docker run -p <host-port>:<application-port> <name>
docker run -v <file-in-host>:<file-in-container> <name>
docker run --env-file <filename> <name>
# run image, run `ls -la`, remove container
docker run --rm <image-name> sh -c "ls -la"

docker kill <container-id>

docker compose up --build
docker compose up -d
docker compose up
docker compose down
docker compose -f <filename> up
docker compose up --scale <name>=N
docker compose port --index <i> <name> <port>
docker compose ps -q <name>

docker volume ls

docker container run -d <image-name>
# prune stopped containers
docker container prune

docker ps --all

docker exec -it <container name or id> bash
# jump into running container as a root
docker exec -u root -it <container name or id> bash

docker login

docker tag <local-image>:tag <username>/<remote-name>:tag

docker push <username>/<remote-name>:tag

docker history [options] <image>

docker inspect <name>
docker container inspect <name> | grep -A 5 Mounts

docker logs -f <name>
docker start <name>
docker attach [--no-stdin] <name>

docker search <image-name>

docker cp ./file <name>:/path
docker diff <name>
docker commit <name> <new-name>

# vi mode
docker exec -it <name> bash -c "echo 'set -o vi'>~/.bashrc && echo 'set editing-mode vi'>~/.inputrc && bash"
```

```bash
# .dockerignore
.gitignore
node_modules
Dockerfile
```
