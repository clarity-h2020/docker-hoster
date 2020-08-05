# docker-hoster

[Hoster](https://github.com/dvddarias/docker-hoster) is simple "etc/hosts" file injection tool to resolve names of local Docker containers on the host. Instead of exposing ports of a container on the docker host (Localhost), you can access the container by its name directly, e.g. csis-postgis:5432 for accessing the [CSIS Drupal](https://github.com/clarity-h2020/docker-drupal/) postgis db.

## Description

hoster is intended to run in a Docker container. The `docker.sock` is mounted to allow hoster to listen for Docker events and automatically register containers IP. Hoster inserts into the host's `/etc/hosts` file an entry per running container and keeps the file updated with any started/stoped container.

### Container Registration

Hoster provides by default the entries `<container name>, <hostname>, <container id>` for each container and the aliases for each network. Containers are automatically registered when they start, and removed when they die.

For example, the following container would be available via DNS as `myname`, `myhostname`, `et54rfgt567` and `myserver.com`:

    docker run -d \
        --name myname \
        --hostname myhostname \
        --network somenetwork --network-alias "myserver.com" \
        mycontainer

If you need more features like **systemd interation** and **dns forwarding** please check [resolvable](https://hub.docker.com/r/mgood/resolvable/)

## Implementation

CLARTIY docker-hoster is based on the [vddarias/docker-hoster](https://github.com/dvddarias/docker-hoster) repository and has among others been cloned to add a [docker compose file](https://github.com/clarity-h2020/docker-hoster/blob/clarity/docker-compose.yml).

## Deployment

CLARTIY docker-hoster is deployed as docker container `hoster`  on [CSIS Development System](https://github.com/clarity-h2020/csis#csis-development-system) and [CSIS Production System](https://github.com/clarity-h2020/csis#csis-production-system) virtual servers. 

### Starting the container

The docker-hoster containers can be started with

```sh
cd /docker/000-hoster
docker-compose up -d --force-recreate --remove-orphans
```

## License
 
MIT © [dvddarias](https://github.com/dvddarias/)
