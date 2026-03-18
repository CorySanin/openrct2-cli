# corysanin/openrct2-cli

Identical to [openrct2/openrct2-cli](https://hub.docker.com/r/openrct2/openrct2-cli), with additional images to choose from and the `develop` tags only gets updated when the network version changes. Essentially this skips all updates that don't impact OpenRCT2 server functionality. All builds are automated, latest versions are typically pushed 30 minutes after they hit OpenRCT2's code repository.

## OpenRCT2

An open-source re-implementation of RollerCoaster Tycoon 2. A construction and management simulation video game that simulates amusement park management.

This repository contains `Dockerfile` definitions for the command line version OpenRCT2. These images should be used for hosting multiplayer servers or executing commands such as generating screenshots.

You can pull and test the latest develop version by running:
```
$ docker run --rm ghcr.io/corysanin/openrct2-cli:develop --version
```

## Multiplayer

To host a multiplayer server, create a container that exposes the desired port and pass in a URL to the park to load. For example:

```
$ docker run --rm -p 11753:11753 -it corysanin/openrct2-cli host https://ffa-tycoon.com/parks/sandbox
```

All configuraion data is stored inside the container. If you want to persit it outside the container, you can mount it to a volume or your local filesystem. Mounting your local filesystem also allows you to read and write saved games locally. For example:

```
$ docker run --rm -p 11753:11753 -v /home/me/openrct2-config:/home/openrct2/.config/OpenRCT2 -it corysanin/openrct2-cli host /home/openrct2/.config/OpenRCT2/save/mypark.park
```

The command above will mount the OpenRCT2 user / config directory inside the container to a directory on your local filesystem. This will allow you to persist and edit the configuration, saved games etc. locally.

It will then host a new server and load the saved game `mypark.park` located in the mounted directory under the save sub-directory.

## Image

- [`ghcr.io/corysanin/openrct2-cli`](https://github.com/CorySanin/openrct2-cli/pkgs/container/openrct2-cli)
- [`docker.io/corysanin/openrct2-cli`](https://hub.docker.com/r/corysanin/openrct2-cli)

## Tags

- [`develop-alpine`, `develop`](https://github.com/CorySanin/openrct2-cli/blob/more/develop/cli-alpine/Dockerfile)
- [`develop-ubuntu`](https://github.com/CorySanin/openrct2-cli/blob/more/develop/cli-ubuntu/Dockerfile)
- [`latest-alpine`, `latest`](https://github.com/CorySanin/openrct2-cli/blob/more/develop/cli-alpine/Dockerfile)
- [`latest-ubuntu`](https://github.com/CorySanin/openrct2-cli/blob/more/develop/cli-ubuntu/Dockerfile)
