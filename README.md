# Ladder99 Demo

Mazak has a set of CNC machines connected to MTConnect Agents - the data for one of the machines can be viewed here - https://demo-north.ladder99.com.

![](assets/dashboard.jpg)

The XML underlying the dashboard is available here - http://mtconnect.mazakcorp.com:5717/current.

## Architecture

![](assets/architecture.png)

Ladder99 is used to read data from an MTConnect Agent - the data is written to a SQL database (Postgres), which is then read by the dashboard (Grafana).

Ladder99 is a free and open-source pipeline for reading data from devices and MTConnect Agents and transforming it to easily digestible dashboards. For more information see https://github.com/Ladder99/ladder99-ce.

## Links

Dashboard (Grafana)
https://demo-north.ladder99.com

SQL access (pgAdmin)
https://demo-north-beaver.ladder99.com

Monitor (Traefik)
https://demo-north-monitor.ladder99.com

More Mazak Agents
http://mtconnect.mazakcorp.com

## Setup

This setup uses a Raspberry Pi to host the Ladder99 pipeline (Relay, Postgres, Grafana), SQL access (pgadmin), and a reverse proxy (Traefik). 

Login to the pi console here - https://teleport.ladder99.com/web/cluster/teleport.ladder99.com/console/node/d66da91e-e6f9-4599-84b5-3de313a2cd10/root

Login as root then drop down to mriiot with `su -- mriiot`, then `cd ~`.

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

## Traefik

Traefik is a reverse proxy that exposes services to the internet. See the compose-overrides.yaml for things like this - 

    labels:
      - traefik.enable=true
      - traefik.http.routers.agent.rule=Host(`demo-north-agent.ladder99.com`)
      - traefik.http.routers.agent.tls=true
      - traefik.http.routers.agent.tls.certresolver=lets-encrypt
      - traefik.port=8888

Note the '.agent.' in the keys - it just needs to be something unique for each service exposed.

## Diagram source

https://docs.google.com/drawings/d/1bBneF6XSqm4Qls_OiLGLMFbBTbfJwSQ-BV3SmlJ-H90/edit?usp=sharing

## License

Apache 2.0
