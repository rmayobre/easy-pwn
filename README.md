# Easy Private Wireguard Network (easy pwn)
A private wireguard network stack (`PWN`) with Unbound DNS, PiHold ad blocking, and a Web UI for managing Wireguard peer connections.

## Addresses
* 10.2.0.0/24 - IP Address range for Docker Containers in PWN network.
* 10.6.0.0/24 - IP Address range for peer connections to Wireguard server.
* http://10.2.0.100/admin - Pi Hole Web UI
* http://10.2.0.3:51821 - Easy Wireguard Web UI
* http://10.2.0.4:9000 - Portainer Web UI
* 10.2.0.5:9001 - Portainer Agent

## Requirements
You must run Linux with a kernel that has support for Wireguard. Most Linux distros now come with Wireguard by default, however, some do not. CentOS 8 distros, for example, do not come with Wireguard installed. If you are running a CentOS 8 distro (Rocky Linux 8, AlmaLinux, CentOS 8, etc.), try running this [script](https://github.com/rmayobre/scripted-selfhost/tree/main/scripts/wireguard-centos8) to install the Wireguard Kernel modules.

You will also need to install the [Docker Engine](https://docs.docker.com/engine/install/) and [docker-compose](https://docs.docker.com/compose/install/).

## Before Installing
Change the password to the `wireguard` service:
```yaml
services:
    wireguard:
    ...
    environment:
        ...
        - PASSWORD=admin # Update this - comment out for no password requirements.
        ...
    ...
...
```

Change the password to the `pihole` service:
```yaml
services:
    wireguard:
    ...
    environment:
        ...
        WEBPASSWORD: "" # Create a password, otherwise, no password required to access.
        ...
    ...
...
```

## Installation

Navigate to this repo's directory and run docker compose:
```sh
docker-compose up -d
```

### Access LocalHost
For the initial installation and connection to the PWN network, you will need access the docker host's localhost. If you are using a remote server or VPS, I recommend SSH port fowarding to access the Wireguard UI (wg-easy).

SSH port forward port number 51821 to your localhost to gain access to the Wireguard UI (wg-easy). 

SSH Config file template (~/.ssh/config):
```sh
Host myserver # Change to a reasonable host alias
   HostName 256.256.256.256 # Change to remote server's IP Address.
   User myuser # Change to your username
   LocalForward 51821 localhost:51821 # Forwards port 51821 to your localhost.
```

Now ssh into your server:
```sh
ssh myserver
```

Once you're connected, go to your browser and go to [localhost:51821](localhost:51821) and log into Wireguard. Use the password declared in the `docker-compose.yml`.

Create a client profile for your computer to use and connect to the Wireguard server.

Once you are connected to the wireguard server, you can access the Wireguard UI (wg-easy) as well as portainer and Pi hole via Wireguard connection. See `Addresses` at the top for the links.

## Portainer Configuration
Go to `Portainer` (http://10.2.0.4:9000) and create a profile. After creating a profile, connect to the `Protainer Agent`. Connect to the the following endpoint: `10.2.0.5:9001`. Once connected, Portainer is ready to use.