# Client setup for Mazak demo

Mazak has a set of CNC machines connected to MTConnect Agents here http://mtconnect.mazakcorp.com/.

This setup uses a Digital Ocean droplet to host the Ladder99 pipeline (Relay, Postgres, Grafana), and a reverse proxy (Traefik).

## Diagram

![](assets/architecture.png)

https://docs.google.com/drawings/d/1bBneF6XSqm4Qls_OiLGLMFbBTbfJwSQ-BV3SmlJ-H90/edit?usp=sharing

## Links

Grafana
https://demo-north.ladder99.com

Traefik monitor
https://demo-north-monitor.ladder99.com

Droplet console
https://cloud.digitalocean.com/droplets/285467341/access

If you login as root, drop down to user with `su - user`.

## Setup

for use with Traefik so we can serve Grafana on port 443

so the idea is that traefik container will be the only one with port 80 and 443 exposed on an external docker network

must do this manually before running ./start!

    docker network create web

got compose-overrides.yaml working correctly -
in ladder99-ce

    ./start mazak traefik

then visit traefik at

https://demo-north-monitor.ladder99.com/dashboard/

grafana on
https://demo-north.ladder99.com/dashboard/

Setup based on
https://www.digitalocean.com/community/tutorials/how-to-use-traefik-v2-as-a-reverse-proxy-for-docker-containers-on-ubuntu-20-04
