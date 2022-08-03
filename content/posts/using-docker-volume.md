---
title: "Working with a docker volume"
date: 2022-07-08T07:51:52+02:00
draft: true
---

Working with volumes to maintain application data beyond a containers lifetime/existence.
this means the volume exists beyond existence on container

volumes can enable sharing between host & container, & between containers

creating a volume during container creation / create a container attached to a volume - volume is creating in process

mounting a volume as read-only

attaching to existing data : practical use - the postgres database PGDATA location

```zsh
docker run -d -v adfaf/adfadf -
```
note path name

find volume location on host
```zsh
docker inspect container-name
```

name a bloody string, & location specified by docker
something like ' /var/lib/docker/volumes/17185f0681e44ba9865017cdb7bbd5cae3df26ee05ad20feb6958378dc135679/_data'

Creating a named volume
```zsh
docker volume create --name my_volume
```

attaching to the named volume

note option assigned to -v, its no longer a path


creating own volume location

using a container as a volume

unfotunately feature to have a named volume with custom location not available
can't attach a volume to an existing container

creating inside Dockerfile / Composer

basic volume management

create it
inspect it
delete it

volume ownership

## references
<https://www.ionos.com/digitalguide/server/know-how/docker-container-volumes/>
