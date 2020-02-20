# traefik

A lightweight HTTP reverse proxy, based on [Traefik](https://traefik.io/).

## Usage

Copy `.env.example` to `.env` and set a new value for `COMPOSE_PROJECT_NAME` if needed.

Run `docker-compose`.

```
docker-compose up
```

Run `docker-compose` with the `-d` (detached) parameter if you want the service to run in the background.

```
docker-compose up -d
```

Usually you do not need more than one Traefik instance running on the same machine because Traefik ties directly into Docker to automatically handle all running containers.

## Configuring access to the Traefik public network

In order for your other services to gain access to the generated public network, through which Traefik is listening for incoming requests, you need to configure them properly.

First add the following service level `networks` configuration to each of the services that will be using Traefik.

```
networks:
    - public
```

After that add a top level `networks` section which will allow containers to gain access to the public Traefik network.

```
networks:
    public:
        external: NETWORK_NAME
        driver: bridge
```

The value for `NETWORK_NAME` should be replaced with whatever `COMPOSE_PROJECT_NAME_public` yields in this project (for the default value of `COMPOSE_PROJECT_NAME` it would be `docker-services-traefik_public`).

Finally, explicitely allow your services to access Traefik by adding the following line to the `labels` section.

```
labels:
    - "traefik.enable=true"
```

Here is an example of of a `docker-compose.yml` file with the above configuration.

```
version: '3.2'
services:
    my-awesome-service:
        networks:
            - public
        labels:
            - "traefik.enable=true"

networks:
    public:
        external: docker-services-traefik_public
        driver: bridge
```

## Default container restart policy

Please note that the `traefik` service has been configured with the `unless-stopped` restart policy which means that it will continue to run until manually stopped, even after the machine has been restarted.

If you do not want the service to restart automatically you can disable that behavior by simply removing the `restart: unless-stopped` line from `docker-compose.yml` or by choosing a different restart policy from the list which you can find [here](https://docs.docker.com/compose/compose-file/#restart).

## Requirements

Docker 17.12.0 or newer.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
