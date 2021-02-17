# Graylog in docker-compose
This `docker-compose.yml` runs all the required processes for a Graylog setup on multiple docker containers.

The following processes are run in their own docker containers

* mongodb 
* elasticsearch 
* graylog

## Create SHA-256 hash of our password to replace `GRAYLOG_ROOT_PASSWORD_SHA2` before using `docker-compose up --build -d`
```bash
> echo -n 'password' | sha256sum | awk '{ print $1 }'
5e884898da28047151d0e56f8dc6292773603d0d6aabbdd62a11ef721d1542d8
```

## Change the password secret
```bash
GRAYLOG_PASSWORD_SECRET=somepasswordpepper
```

## Replace the localhost with the host IP
Determine the host IP address and replace the following ip callouts with the host address. 

```bash
GRAYLOG_HTTP_EXTERNAL_URI=http://127.0.0.1:9000/
GRAYLOG_HTTP_PUBLISH_URI=http://127.0.0.1:9000/
```

## Run 
```bash
docker-compose up --build --remove-orphans -d
```
Open [https://$Machine_IP:9000/](https://$Machine_IP:9000/) and use the login. (It may take a minute for the graylog server to come online)
```bash
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