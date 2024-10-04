# Docker Compose for connecting to Forti VPN using Docker and Tailscale

## Motivation

1. The FortiClient service in the container is designed for users to connect to a VPN with proxy support, which is important in situations where bypassing network restrictions or routing traffic through a specific proxy server is required.
2. Tailscale is used to create a secure network infrastructure that connects all devices (containers, servers, workstations) through secure tunnels. It is especially useful for remote access to resources and building distributed networks.```


## .env

```dotenv
VPNADDR=vpn.example.com:212
VPNUSER=username
VPNPASS=password
VPNTIMEOUT=60
TS_HOSTNAME=vpn-in-docker
TS_STATE_DIR=/var/run/tailscale
```

## Initialization

1. Clone this repository.
2. Edit the `.env` file.
3. Run:
```bash 
docker-compose up -d
```
4. Open the Tailscale logs and follow the link for authorization:
```bash 
docker logs tailscale
```
5. Download and log in to Tailscale for your platform on the [official website](https://tailscale.com/download).

## Testing

Testing via curl:

```bash
curl -x socks5://vpn-in-docker:1080 example.com
```

## SSH

Configuration for SSH and GIT.

```bash
cat ~/ssh/config
```

```bash
Host example.com
    Identityfile ~/.ssh/id_rsa
    ProxyCommand /usr/bin/nc -X 5 -x vpn-in-docker:1080 %h %p
```
