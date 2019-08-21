# cloud9

My image is based on [linuxserver/cloud9](https://github.com/linuxserver/docker-cloud9), but i add SSH client for remote connection

[Cloud9](https://github.com/c9/core) Cloud9 is a complete web based IDE with terminal access. This container is for running their core SDK locally and developing plugins.

## Version Tags

This image provides various versions that are available via tags. `latest` tag usually provides the latest stable version. Others are considered under development and caution must be exercised when using them.

| Tag | Description |
| :----: | --- |
| latest | Docker and Compose environment pre-installed |

## Usage

Here are some example snippets to help you get started creating a container.

### docker

```
docker create \
  --name=cloud9 \
  -e PUID=1000 \
  -e PGID=1000 \
  -e TZ=Europe/London \
  -e GITURL=https://github.com/linuxserver/docker-cloud9.git `#optional` \
  -p 8000:8000 \
  -v <path to your code>:/code `#optional` \
  -v /var/run/docker.sock:/var/run/docker.sock `#optional` \
  --restart unless-stopped \
  linuxserver/cloud9
```


### docker-compose

Compatible with docker-compose v2 schemas.

```
---
version: "2"
services:
  cloud9:
    image: linuxserver/cloud9
    container_name: cloud9
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/London
      - GITURL=https://github.com/linuxserver/docker-cloud9.git #optional
    volumes:
      - <path to your code>:/code #optional
      - /var/run/docker.sock:/var/run/docker.sock #optional
    ports:
      - 8000:8000
    restart: unless-stopped
```

## Parameters

Container images are configured using parameters passed at runtime (such as those above). These parameters are separated by a colon and indicate `<external>:<internal>` respectively. For example, `-p 8080:80` would expose port `80` from inside the container to be accessible from the host's IP on port `8080` outside the container.

| Parameter | Function |
| :----: | --- |
| `-p 8000` | The port for the Cloud9 web interface |
| `-e PUID=1000` | for UserID - see below for explanation |
| `-e PGID=1000` | for GroupID - see below for explanation |
| `-e TZ=Europe/London` | Specify a timezone to use EG Europe/London, this is required for Radarr |
| `-e GITURL=https://github.com/linuxserver/docker-cloud9.git` | Specify a git repo to checkout on first startup |
| `-v /code` | Optionally if you want to mount up a local folder of code instead of checking out |
| `-v /var/run/docker.sock` | Needed if you plan to use Docker or compose commands |

## User / Group Identifiers

When using volumes (`-v` flags) permissions issues can arise between the host OS and the container, we avoid this issue by allowing you to specify the user `PUID` and group `PGID`.

Ensure any volume directories on the host are owned by the same user you specify and any permissions issues will vanish like magic.

In this instance `PUID=1000` and `PGID=1000`, to find yours use `id user` as below:

```
  $ id username
    uid=1000(dockeruser) gid=1000(dockergroup) groups=1000(dockergroup)
```


&nbsp;
## Application Setup

Access the webui at http://your-ip:8000, for more information check out [here](https://docs.c9.io/docs).



## Support Info

* Shell access whilst the container is running: `docker exec -it cloud9 /bin/bash`
* To monitor the logs of the container in realtime: `docker logs -f cloud9`
* container version number 
  * `docker inspect -f '{{ index .Config.Labels "build_version" }}' cloud9`
* image version number
  * `docker inspect -f '{{ index .Config.Labels "build_version" }}' linuxserver/cloud9`

## Updating Info

Most of our images are static, versioned, and require an image update and container recreation to update the app inside. With some exceptions (ie. nextcloud, plex), we do not recommend or support updating apps inside the container. Please consult the [Application Setup](#application-setup) section above to see if it is recommended for the image.  
  
Below are the instructions for updating containers:  
  
### Via Docker Run/Create
* Update the image: `docker pull linuxserver/cloud9`
* Stop the running container: `docker stop cloud9`
* Delete the container: `docker rm cloud9`
* Recreate a new container with the same docker create parameters as instructed above (if mapped correctly to a host folder, your `/config` folder and settings will be preserved)
* Start the new container: `docker start cloud9`
* You can also remove the old dangling images: `docker image prune`

### Via Docker Compose
* Update all images: `docker-compose pull`
  * or update a single image: `docker-compose pull cloud9`
* Let compose update all containers as necessary: `docker-compose up -d`
  * or update a single container: `docker-compose up -d cloud9`
* You can also remove the old dangling images: `docker image prune`

### Via Watchtower auto-updater (especially useful if you don't remember the original parameters)
* Pull the latest image at its tag and replace it with the same env variables in one run:
  ```
  docker run --rm \
  -v /var/run/docker.sock:/var/run/docker.sock \
  containrrr/watchtower \
  --run-once cloud9
  ```

**Note:** We do not endorse the use of Watchtower as a solution to automated updates of existing Docker containers. In fact we generally discourage automated updates. However, this is a useful tool for one-time manual updates of containers where you have forgotten the original parameters. In the long term, we highly recommend using Docker Compose.

* You can also remove the old dangling images: `docker image prune`

