# docker-archivebox

The official Docker image for [ArchiveBox](https://github.com/ArchiveBox/ArchiveBox), the self-hosted internet archiving solution.

https://hub.docker.com/r/archivebox/archivebox
```bash
docker pull archivebox/archivebox:dev
```

**Tags available:**

It's recommended to use either `:master` (stable, all architectures) or `:dev` (beta/unstable).

- `:latest` (the default stable tag, 1:1 with `:master`, only built for `amd64`)
- `:dev`/`:master`/`:<branchname>` (tags for each git branch, built for `amd64`, `arm64`, `arm/v7`)
- `sha-2c7be14`/`:sha-<commitid>` (tags for each git commit, built for `amd64`, `arm64`, `arm/v7`)

*For a full list of the published images: https://hub.docker.com/r/archivebox/archivebox/tags*

<img width="500px" alt="Docker Hub Screenshot" src="https://user-images.githubusercontent.com/511499/147287184-6f1201f8-6827-4002-a6a3-3aae7eb859d4.png">

## Quickstart

**Pull the image:**
```bash
docker pull archivebox/archivebox:dev
docker image ls archivebox/archivebox:dev
```


**Try it out:**
```bash
docker run archivebox/archivebox version

mkdir data && cd data
docker run -v $PWD:/data -it archivebox/archivebox init
docker run -v $PWD:/data -it archivebox/archivebox add 'https://example.com'
docker run -v $PWD:/data -it -p 8000:8000 archivebox/archivebox server 0.0.0.0:8000
```

---
**Kubernetes:**

archivebox.yml contains an example manifest used with rook-ceph-rbd and metallb.

Use as-is, or edit to your needs, objects will be created in namespace: `archivebox`

```bash
kubectl apply -f archivebox.yml
```
---

Tested on amd64/x86, arm64, and armv7, should work on all systems that support Docker.

---

## Docker Compose

**`docker-compose.yml`:**
```yaml
version: '3.9'

services:
    archivebox:
        image: 'archivebox/archivebox:master'
        command: server --quick-init 0.0.0.0:8000
        ports:
            - 8000:8000
        environment:
            # add any ArchiveBox config options you want here
            - ALLOWED_HOSTS=*
            - MEDIA_MAX_SIZE=750m
        volumes:
            - ./data:/data
```

```bash
docker-compose up

# Usage: docker-compose run archivebox [subcommand] [...args]

docker-compose run archivebox setup --init
docker-compose run archivebox add --depth=1 'https://example.com'
...
```

To enable Sonic full-text search backend and other optional extras, see: https://github.com/ArchiveBox/ArchiveBox/blob/dev/docker-compose.yml#L30

---

## Development

The image is built using `docker`: https://docs.docker.com/get-docker/ and hosted on Docker Hub & Github Packages: https://hub.docker.com/r/archivebox.

https://hub.docker.com/r/archivebox/archivebox

The config file / package definition is here: [`ArchiveBox/Dockerfile`](https://github.com/ArchiveBox/ArchiveBox/blob/master/Dockerfile).

To build this package, make sure you are in the ArchiveBox main repo first.

```bash
cd ArchiveBox/
git pull --recurse-submodules

# Build the docker image
./bin/build_docker.sh

# Push the image to Docker Hub & Github Packages
./bin/release.sh
```

**Inspecting the image:**

```bash
docker image ls archivebox/archivebox

docker image inspect <image id>  # view image details
docker image history <image id>  # view image layer sizes
```
---


*Please note: The old image at [`nikisweeting/archivebox`](https://hub.docker.com/r/nikisweeting/archivebox) is deprecated, use [`archivebox/archivebox`](https://hub.docker.com/r/archivebox/archivebox) instead.*

