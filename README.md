# zerotier-docker

## zerotier

### Description

This is a container based on a lightweight Alpine Linux image and a copy of ZeroTier One. It's designed to allow you to run ZeroTier One as a service on container-oriented distributions like Fedora CoreOS, though it should work on any Linux system with Docker or Podman.

### Start

```bash
docker run --name zerotier-one -d --restart always --device=/dev/net/tun --net=host --cap-add=NET_ADMIN \
  --cap-add=SYS_ADMIN -v /var/lib/zerotier-one:/var/lib/zerotier-one jerryin/zerotier
```

This runs `jerryin/zerotier` in a container with special network admin permissions and with access to the host's network stack (no network isolation) and `/dev/net/tun` to create tun/tap devices. This will allow it to create zt# interfaces on the host the way a copy of ZeroTier One running on the host would normally be able to.

It also mounts `/var/lib/zerotier-one` to `/var/lib/zerotier-one` inside the container, allowing your service container to persist its state across restarts of the container itself. If you don't do this it'll generate a new identity every time. You can put the actual data somewhere other than `/var/lib/zerotier-one` if you want.

To join a zerotier network you can use

```bash
docker exec zerotier-one zerotier-cli join 8056c2e21c000001
```

Or create an empty file with the network as name

```bash
/var/lib/zerotier-one/networks.d/8056c2e21c000001.conf
```

## zerotier-moon

A docker image to create ZeroTier moon in one setp.

### Usage

#### Pull the image

```bash
docker pull jerryin/zerotier-moon
```

#### Start a container

```bash
docker run --name zerotier-moon -d --restart always -p 9993:9993/udp jerryin/zerotier-moon -4 1.2.3.4
```

Replace `1.2.3.4` with your moon's IP.

To show your moon id, run

```bash
docker logs zerotier-moon
```

**Notice:**
When creating a new container, a new moon id will be generated. To persist the identity when creating a new container, see **Mount ZeroTier conf folder** below.

### Advanced usage

#### Manage ZeroTier

```bash
docker exec zerotier-moon zerotier-cli
```

#### Mount ZeroTier conf folder

```bash
docker run --name zerotier-moon -d -p 9993:9993/udp -v ~/somewhere:/var/lib/zerotier-one jerryin/zerotier-moon -4 1.2.3.4
```

This will mount `~/somewhere` to `/var/lib/zerotier-one` inside the container, allowing your ZeroTier moon to presist the same moon id.  If you don't do this, when you start a new container, a new moon id will be generated.

#### IPv6 support

```bash
docker run --name zerotier-moon -d -p 9993:9993/udp jerryin/zerotier-moon -4 1.2.3.4 -6 2001:abcd:abcd::1
```

Replace `1.2.3.4`, `2001:abcd:abcd::1` with your moon's IP. You can remove `-4` option in pure IPv6 environment.

#### Docker compose

```yml
version: '3'

services:
  zerotier-moon:
    image: jerryin/zerotier-moon
    restart: always
    container_name: zerotier-moon
    ports:
      - "9993:9993/udp"
    volumes:
      - ./zerotier-one:/var/lib/zerotier-one
    command: -4 1.2.3.4

```

## Build docker image

```bash
# create builder
docker buildx create --use --name mybuilder
docker buildx inspect mybuilder --bootstrap

# build zerotier
docker buildx build -t jerryin/zerotier --platform=linux/arm64,linux/amd64 . --push
# build zerotier-moon
docker buildx build -t jerryin/zerotier-moon --platform=linux/arm64,linux/amd64 . --push
```
