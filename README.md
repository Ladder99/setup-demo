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

SQL access (Hue)
https://demo-north-beaver.ladder99.com

Monitor (Traefik)
https://demo-north-monitor.ladder99.com

More Mazak Agents
http://mtconnect.mazakcorp.com

## Setup

For detailed setup instructions, see [SETUP.md](SETUP.md).

## License

Apache 2.0
