## About

This setup uses a Raspberry Pi to host the Ladder99 pipeline (Relay, Postgres, Grafana), SQL access (pgadmin), and a reverse proxy (Traefik). The Traefik container will be the only one with port 80 and 443 exposed (?).

## Diagram source

https://docs.google.com/drawings/d/1bBneF6XSqm4Qls_OiLGLMFbBTbfJwSQ-BV3SmlJ-H90/edit?usp=sharing

## Setup

Login to the pi console here - https://teleport.ladder99.com/web/cluster/teleport.ladder99.com/console/node/d66da91e-e6f9-4599-84b5-3de313a2cd10/root

Login as root then drop down to mriiot with `su - mriiot`.

Make a directory ladder99

    mkdir ladder99
    cd ladder99

Clone this repo and ladder99-ce there

    git clone https://github.com/Ladder99/ladder99-ce
    git clone https://github.com/Ladder99/client-demo
    cd ladder99-ce
    git checkout develop

Start all Docker services (Grafana, Postgres, Relay, pgadmin, Traefik) -

    ./start demo all

- you'll be asked to edit an .env file - be sure to set the Postgres password, 
and add the MTConnect Agents you want to query - e.g. 

    AGENT_ENDPOINTS=http://mtconnect.mazakcorp.com:5717


## Status

See status of services with

    docker ps

## Postgres

To access the database console

    docker exec -it postgres bash

then within the postgres container

    psql -U postgres

now can enter SQL and Postgres commands

    psql (13.3)
    Type "help" for help.
    postgres=#

eg

    postgres=# \d
                    List of relations
    Schema |       Name        |   Type   |  Owner
    --------+-------------------+----------+----------
    public | bins              | table    | postgres
    public | dataitems         | view     | postgres
    public | devices           | view     | postgres
    public | edges             | table    | postgres
    public | history           | table    | postgres
    public | history_all       | view     | postgres
    public | history_float     | view     | postgres
    public | history_text      | view     | postgres
    public | meta              | table    | postgres
    public | metrics           | view     | postgres
    public | nodes             | table    | postgres
    public | nodes_node_id_seq | sequence | postgres

## Update

To update code when github repos are updated

    cd ~/ladder99/ladder99-ce
    ./update demo

this will update both ladder99-ce and client-demo repos.

If you need to update the running Grafana dashboard,

    docker stop grafana
    ./start demo grafana

To restart the Relay service,

    docker stop relay
    ./start demo relay

# Traefik

Traefik is a reverse proxy that exposes services to the internet. See the compose-overrides.yaml for things like this - 

    labels:
      - traefik.enable=true
      - traefik.http.routers.agent.rule=Host(`demo-north-agent.ladder99.com`)
      - traefik.http.routers.agent.tls=true
      - traefik.http.routers.agent.tls.certresolver=lets-encrypt
      - traefik.port=8888

Note the '.agent.' in the keys - it just needs to be something unique for each service exposed.

