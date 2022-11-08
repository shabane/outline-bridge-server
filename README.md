# Outline Bridge Server

This repository contains V2Ray Docker Compose files to run a bridge server for the Outline proxy.
It helps Outline proxy to work in highly restricted networks.

## Documentation

### Outline

[Outline](https://getoutline.org) is a Shadowsocks-based proxy created by Google.
It provides high-quality Shadowsocks servers, clients, and managers.

It usually works as below.

```
(Outline client) <-> [Outline Server] <-> (Internet)
```

### The Bridge Server

The bridge server will be inserted between clients and a server to connect the clients to the server
in networks where this connection is not possible directly.
It uses the V2Ray proxy to pass the incoming Shadowsocks traffic from Outline clients to the specified Outline server.

The bridge (V2Ray) server changes the flow as below.

```
(Outline client) <-> [V2Ray server] <-> [Outline server] <-> (Internet)
```

### Setup Outline

Read the [Outline official documentation](https://getoutline.org) to set up an Outline server using the Outline Manager application.
The Outline Manager creates and manages Outline servers and users.

### Setup the bridge server

Follow these steps to run the V2Ray proxy server.

1. Install Docker and Docker-compose.
1. Clone this repository into the bridge server.
1. Run `./setup.py` script. It gets the following inputs:
    1. `Outline Server Hostname`: Find it in Outline Manager > Server > Settings > Hostname
    1. `Outline Server Port`: Find it in Outline Manager > Server > Settings > Port
    1. `Bridge Port`: Enter a port for V2Ray (Lazy option: enter the same Outline server port)
1. Run `docker-compose up -d`.

### Use the bridge server

The Outline Manager generates a Outline (Shadowsocks) link for each user so they can import the link into an Outline client app.
This is a sample link generated by the Outline Manager:

```
ss://Y2hhY2hhMjAtaWV0Zi1wb2x5MTMwNTpoZjV4ZFh4YUhya1k@13.13.13.13:1080/?outline=1
```

To make user use the bridge server instead of Outline server,
you must change the hostname (13.13.13.13) and port (1080) in the original link above to the bridge hostname and port.
The `./convert.py` script can handle this link conversion.

Run the `./convert.py` script.
* It gets the following inputs:
   1. `Bridge Hostname`: Enter the bridge server IP address or domain.
   1. `Original Outline Link`: Enter the original ss:// link generated by Outline Manager.
* It prints the following outputs:
   1. `New Outline Link`: The new link with bridge IP/Domain and port.
   1. `New Invitation Link`: The new invitation link to share with your users.

### Docker images

* GitHub:
    * Image: ```ghcr.io/getimages/v2fly-core:v4.45.2```
    * URL: https://github.com/orgs/getimages/packages/container/package/v2fly-core
    * Digest: `sha256:289fc9451f21a265f95615e29f05ea23bc32026db152863eee317738813521d7`
* Docker Hub:
    * Image: ```v2fly/v2fly-core:v4.45.2```
    * URL: https://hub.docker.com/r/v2fly/v2fly-core/tags
    * Digest: `sha256:289fc9451f21a265f95615e29f05ea23bc32026db152863eee317738813521d7`

## More

* [V2Ray Docker Compose (Bridge and Upstream Servers)](https://github.com/miladrahimi/v2ray-docker-compose)
