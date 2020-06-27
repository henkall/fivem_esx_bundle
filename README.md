[hub]: https://hub.docker.com/r/andruida/fivem
[git]: https://github.com/Andruida/fivem

# [andruida/fivem][hub]

This docker image allows you to run a server for FiveM, a modded GTA multiplayer program.
This image also includes [txAdmin](https://github.com/tabarra/txAdmin), an in-browser server management software.
Upon first run, the configuration is generated in the host mount for the `/config` directory, and for the `/txData` directory (that contains the txAdmin configuration).
The container should be stopped so fivem can be configured to the user requirements in the `server.cfg`.

## Licence Key

A freely obtained licence key is required to use this server, which should be declared as `$LICENCE_KEY`. A tutorial on how to obtain a licence key can be found [here](https://forum.fivem.net/t/explained-how-to-make-add-a-server-key/56120)

## Usage

Use the docker-compose script provided or use the line below:

```sh
docker run -d \
  --name FiveM \
  --restart on-failure \
  -p 30120:30120 \
  -p 30120:30120/udp \
  -p 40120:40120 \
  -e WEB_PORT=40120 \
  -e FIVEM_PORT=30120 \
  -e HOST_UID=1000 \
  -e HOST_GID=1000 \
  -e LICENCE_KEY=<your-license-key> \
  -e SERVER_PROFILE=default \
  -v /volumes/fivem:/config \
  -v /volumes/txAdmin:/txData
  -tid \
  andruida/fivem
```

When the container is running you can access txAdmin on the specified port and login with the username `administrator` and the password `adminadmin`. After login, immediately change login the password.

_It is important that you use `interactive` and `pseudo-tty` options otherwise the container will crash on startup_
See [issue #3](https://github.com/spritsail/fivem/issues/3)

### Environment Varibles

- `LICENSE_KEY` - This is a required variable for the licence key needed to start the server.
- `RCON_PASSWORD` - A password to use for the RCON functionality of the fxserver. If not specified, a random 16 character password is assigned. This is only used upon creation of the default configs
- `WEB_PORT` - Sets up txAdmin to run on this port. If not specified, will use `40120` by default.
- `FIVEM_PORT` - If the server.cfg file needs to be generated, the server will use this port. If not specified, will use `30120` by default.
- `HOST_GID` - The files that are generated by the container will be written with this group ID. You must use numeric IDs. If not specified, will use `0` (root).
- `HOST_UID` - The files that are generated by the container will be written with this user ID. You must use numeric IDs. If not specified, will use `0` (root).
- `SERVER_PROFILE` - profile name used by txAdmin. If not specified, will use `dev_server`.

## Credits

 - This image is based on the [spritsail/fivem](https://hub.docker.com/r/spritsail/fivem) image. Thanks to **Spritsail** !
 - Thanks to **tabarra** as the creator and maintainer of the [txAdmin](https://github.com/tabarra/txAdmin) repository!