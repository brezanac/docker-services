# portainer

[Portainer](https://www.portainer.io/) is a web based tool for maintaining and managing Docker environments.

## Usage ##

Copy `.env.example` to `.env` and set a new value for `COMPOSE_PROJECT_NAME` if needed.

Run `docker-compose` with the `-d` (detached) argument to make the container run in the background.

```
docker-compose up --build -d
```

If you do not want to run Portainer in the background simply omit the `-d` argument.

```
docker-compose up --build
```

**NOTE:** you only need one running Portainer instance per machine since it ties directly into Docker, which allows to control all the images, containers etc. from one instance. 

## Default container restart policy ##

Please note that the `portainer` service has been configured with an `unless-stopped` restart policy which means that it will continue to run until manually stopped, even after the machine has been restarted.

If you do not want the service to restart automatically you can disable that behavior by simply removing the `restart: unless-stopped` line from `docker-compose.yml`.

For more details please see the following [link](https://docs.docker.com/compose/compose-file/#restart).

## Requirements

Docker 17.12.0 or newer.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
