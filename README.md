## Initial setup

Create a new swarm:

```bash
docker swarm init
```

Verify docker node and get info:

```bash
docker node ls
```

Node can be removed with the following command:

```bash
docker swarm leave
```


# build nginx reverse proxy image
docker build --tag akourdi/nginx nginx/            # build custom nginx image
```

Verify build image:

```bash
docker image ls
```

The image are created:

- akourdi/nginx



### Nginx as reverse proxy

Create overlay network and don't expose any applications' ports, use nginx to handle proxying instead:

```bash
# create overlay network
docker network create -d overlay swarm-net --attachable
docker network ls


# map nginx service to overlay network and expose ports 80 and 443
docker service create --name nginx-swarm --replicas 1 -p 80:80 -p 443:443 --network swarm-net akourdi/nginx

# show services
docker service ls

# verify
docker ps


Shutdown everything:

```bash
docker service rm nginx-swarm

# check processes (wait a few seconds for graceful shutdown)
docker ps

# remove network
docker network rm swarm-net

# remove docker node
docker swarm leave --force
```
