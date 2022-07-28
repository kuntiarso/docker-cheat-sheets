# *Docker Cheat Sheet*

- **Docker container port forwarding**

  *structure command:*

  `docker container create --name [containerName] --publish [portHost]:[portContainer] [image]:[tag]`

  *command usage:*

  ```bash
  docker container create --name myredis --publish 8080:6379 redis:latest
  ```

- **Docker container log**

  *structure command:*

  `docker container logs -f [containerName]`

  *command usage*:

  ```bash
  docker container logs -f myredis
  ```

- **Docker container exec**

  *structure command:*

  `docker container exec -i -t [containerName] /bin/bash`

  *command usage:*

  ```bash
  docker container exec -i -t myredis /bin/bash
  ```

- **Docker container environment variable**

  *structure command:*

  `docker container create --name [containerName] --env [key]="[value]" --env [key2]="[value2]" [image]:[tag]`

  *command usage:*

  ```bash
  docker container create --name mymongo --publish 27017:27017 --env MONGO_INITDB_ROOT_USERNAME="kuntiarso" --env MONGO_INITDB_ROOT_PASSWORD="password123" mongo:latest
  ```

- **Docker container stats**

  *structure command:*

  `docker container stats`

  *command usage:*

  ```bash
  docker container stats
  ```

- **Docker container resource limit**

  *structure command:*

  `docker container create --name [containerName] --publish [portHost]:[portContainer] --memory [containerMemoryLimit] --cpus [containerCPULimit] [image]:[tag]`

  *command usage:*

  ```bash
  docker container create --name myredis --publish 8080:80 --memory 100m --cpus 0.5 redis:latest
  ```

- **Docker container bind mount**

  *structure command:*

  `docker container create --name [containerName] --mount "type=bind,source=[hostFolder],destination=[containerFolder],readonly" [image]:[tag]`

  *command usage:*

  ```bash
  docker container create --name mymongo --mount "type=bind,source=/Users/kun/mongo-data,destination=/data/db" --publish 27017:27017 --env MONGO_INITDB_ROOT_USERNAME="kuntiarso" --env MONGO_INITDB_ROOT_PASSWORD="password123" mongo:latest
  ```

- **Docker volume**

  *structure command:*

  `docker volume create [volumeName]`

  `docker volume rm [volumeName]`

  *command usage:*

  ```bash
  docker volume create mongodata
  
  docker volume rm mongodata
  ```

- **Docker container volume**

  *structure command:*

  `docker container create --name [containerName] --mount "type=volume,source=[volumeName],destination=[containerFolder],readonly" [image]:[tag]`

  *command usage:*

  ```bash
  docker container create --name mymongo --mount "type=volume,source=mongodata,destination=/data/db,readonly" --publish 27017:27017 --env MONGO_INITDB_ROOT_USERNAME="kuntiarso" --env MONGO_INITDB_ROOT_PASSWORD="password123" mongo:latest
  ```

-  **Docker backup volume**

  *how to backup a volume:*

  ```bash
  docker container stop mymongo
  
  docker container create --name mynginx --mount "type=bind,source=/Users/kuntiarso/backup,destination=/backup" --mount "type=volume,source=mongodata,destination=/data" nginx:latest
  
  docker container start mynginx
  
  docker container exec -i -t mynginx /bin/bash
  ```

- **Docker container run**

  *structure command:*

  `docker container run --rm --name [containerName] --mount "type=bind,source=[sourceDir],destination=[destinationDir]" --mount "type=volume,source=[sourceDir],destination=[destinationDir] [image]:[tag] tar cvf /backup/backup.tar.gz /data"`

  *command usage:*

  ```bash
  docker container run --rm --name myubuntu --mount "type=bind,source=/home/kuntiarso/backups,destination=/backup" --mount "type=volume,source=postgresdata,destination=/data" ubuntu:latest tar cvf /backup/backup.tar.gz /data
  ```

- **Docker restore volume**

  *structure command:*

  `docker container run --rm --name [containerName] --mount "type=bind,source=[sourceDir],destination=[destinationDir]" --mount "type=volume,source=[sourceDir],destination=[destinationDir] [image]:[tag] bash -c "cd /data && tar xvf /backup/backup.tar.gz --strip 1"`

  *command usage:*

  ```bash
  docker volume create pgdatarestore
  
  docker container run --rm --name myubuntu --mount "type=bind,source=/home/kuntiarso/backups,destination=/backup" --mount "type=volume,source=pgdatarestore,destination=/data" ubuntu:latest bash -c "cd /data && tar xvf /backup/backup.tar.gz --strip 1"
  ```

- **Docker network**

  *structure command:*
  
  `docker network create --driver [driverName] [networkName]`
  
  *command usage:*
  
  ```bash
  docker network create --driver bridge mynetwork
  ```
  
- **Docker container with network**

  *structure command:*

  `docker container create --name [containerName] --network [networkName] [image]:[tag]`

  *command usage:*

  ```bash
  docker container create --name mymongo --network mynetwork --env MONGO_INITDB_ROOT_USERNAME="kuntiarso" --env MONGO_INITDB_ROOT_PASSWORD="password123" mongo:latest
  
  docker container create --name mymongoexpress --network mynetwork --publish 8081:8081 --env ME_CONFIG_MONGODB_URL="mongodb://kuntiarso:password123@mymongo:27017/" mongo-express:latest
  ```

- **Docker container disconnect and re-connect network**

  *structure command:*

  `docker network disconnect [networkName] [containerName]`

  `docker network connect [networkName] [containerName]`

  *command usage:*

  ```bash
  docker network disconnect mynetwork mymongo
  
  docker network connect mynetwork mymongo
  ```

- **Docker inspect**

  *structure command:*

  `docker [image/container/volume/network] inspect [the name of image/container/volume/network]`

  *command usage:*

  ```bash
  docker image inspect mongo:latest
  docker container inspect mymongo
  docker volume inspect myvolume
  docker network inspect mynetwork
  ```

- **Docker prune**

  *structure command:*

  `docker [image/container/volume/network] prune`

  `docker system prune`

  *command usage:*

  ```bash
  docker image prune
  docker container prune
  docker volume prune
  docker network prune
  docker system prune
  ```

  

