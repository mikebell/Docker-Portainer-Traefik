# Convivio Portainer/Traefik Docker system

A simple Docker project with a Portainer and Traefik instance.

This system allows traefik to manage networking for multiple running Docker projects via an external network called 'traefik.'

## Usage

### 1) Create a 'traefik' network in Docker

To use this system you must have an external Docker network called 'traefik' created and running:

```
$ docker network create traefik
```

### 2) Set your environment

Copy the `.env.example` file to `.env` and edit as you wish, to name your local Portainer/Traefik tool.

The Traefik container should join the 'traefik' network you created above.

### 3) For networking other Docker systems to this Traefik container

Each Docker container you want to connect to this Traefik — for routing, load balancing or as reverse proxy — should be connected to the external 'traefik' network.

Assuming you are using Docker Compose files:

- the container definition should include this:
    ```
    services:
        …
        examplecontainer:
            …
            networks:
                - traefik
            labels:
                - 'traefik.backend=examplecontainer'
                - 'traefik.port=${PUBLIC_PORT}'
                - 'traefik.frontend.rule=Host:${CONTAINER_BASE_URL}'
                - 'traefik.docker.network=traefik'
    ```
    with the appropriate gaps filled.

- the networks definition should include this:

```
networks:
  traefik:
    external: true
```