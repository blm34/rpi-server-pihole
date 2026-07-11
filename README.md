# Pihole

Docker compose configuration for running pihole on ubuntu server

## Requirements

* Docker
* Docker compose
* An Nginx reverse proxy running as configured [here](https://github.com/blm34/self-hosted-nginx)

## Set Up

Clone this repository

```bash
git clone git@github.com:blm34/rpi-server-pihole.git
```

Copy the file `.env.template` to `.env` and fill in the variables

```bash
cp .env.template .env
```

Open port 53 in the firewall if required

```bash
sudo ufw allow 53
```

Disable the DNS stub listener built into the systemd resolve service. Open the
configuration file:

```bash
sudo vim /etc/systemd/resolved.conf
```

Find the line `#DNSStubListener=yes` and change it to `DNSStubListener=no`

Remove the existing resolve.conf file

```bash
sudo rm /etc/resolv.conf
```

Replace the deleted file with a link to a different version

```bash
sudo ln -s /run/systemd/resolve/resolve.conf /etc/resolv.conf
```

Restart the system-resolvd service to load the changes

```bash
sudo systemctl restart systemd-resolved
```

If [nginx](https://github.com/blm34/rpi-server-nginx) is not set up to access the web UI, uncomment the line exposing
port 80 in `compose.yaml`

Start the container with

```bash
docker compose up -d
```
