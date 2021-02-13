# Graylog in docker-compose
This `docker-compose.yml` runs all the required processes for a Graylog setup on multiple docker containers.

The following processes are run in their own docker containers

* mongodb 
* elasticsearch 
* graylog
* graylog-web 

## Example for using Docker secrets in a Docker Swarm service:

### Create SHA-256 hash of our password before using `docker-compose up`
```bash
> echo -n 'password' | sha256sum | awk '{ print $1 }'
5e884898da28047151d0e56f8dc6292773603d0d6aabbdd62a11ef721d1542d8
```

### Create a Docker secret named "root_password_hash"
```bash
> printf '5e884898da28047151d0e56f8dc6292773603d0d6aabbdd62a11ef721d1542d8' | \
  docker secret create root_password_hash -
nlujwooo5uu6z0m91bmve79uo

> docker secret ls
ID                          NAME                 DRIVER              CREATED             UPDATED
nlujwooo5uu6z0m91bmve79uo   root_password_hash                       34 seconds ago      34 seconds ago
```

### Create Docker Swarm service named "graylog" with access to the secret named "root_password_hash"
```bash
> docker service create --name graylog \
 --secret root_password_hash \
 -e GRAYLOG_ROOT_PASSWORD_SHA2__FILE=/run/secrets/root_password_hash \
 -p 9000:9000 graylog/graylog:3.3
mclk5gm39ingk51s869dc0htz
overall progress: 1 out of 1 tasks
1/1: running   [==================================================>]
verify: Service converged

> docker service ls
ID                  NAME                MODE                REPLICAS            IMAGE               PORTS
mclk5gm39ing        graylog             replicated          1/1                 graylog:3.3      *:9000->9000/tcp
```

## Run 
```bash
docker-compose up --force-recreate --build --remove-orphans -d
```
Open [https://$Machine_IP:9000/](https://$Machine_IP:9000/) and use the login. (It may take a minute for the graylog server to come online)

```
username: admin
password: GRAYLOG_ROOT_PASSWORD_SHA2__FILE
```
## To shutdown the docker-compose instance
``` bash
docker-compose down
```
## Clear all images and containers
```bash
docker system prune -a
```

## List containers
```bash
sudo docker system df -v
```

# License
Apache License 2.0