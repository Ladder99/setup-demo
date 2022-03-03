# Client setup for Mazak demo

Mazak has a set of CNC machines connected to MTConnect Agents - the data for one of the machines can be viewed here - https://demo-north.ladder99.com.

![](assets/dashboard.jpg)

The XML underlying the dashboard is available here - http://mtconnect.mazakcorp.com:5717/current. Others are available also - http://mtconnect.mazakcorp.com.

## Architecture

![](assets/architecture.png)

This setup uses a Digital Ocean droplet to host the Ladder99 pipeline (Relay, Postgres, Grafana), SQL access (Hue), and a reverse proxy (Traefik). The Traefik container will be the only one with port 80 and 443 exposed.

## Links

Dashboard (Grafana)
https://demo-north.ladder99.com

SQL access (Hue)
https://demo-north-beaver.ladder99.com

Monitor (Traefik)
https://demo-north-monitor.ladder99.com

## Setup

For detailed setup instructions, see [SETUP.md](SETUP.md).

## License

Apache 2.0
