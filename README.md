# Pi-hole and unbound Docker Compose on MacVLAN

This repository contains a Docker Compose setup for running [Pi-hole](https://github.com/pi-hole/pi-hole) and [unbound](https://github.com/NLnetLabs/unbound) as a LAN-native service using a macvlan network with a static IP.

---

## Setup

### 1. Create the macvlan 

Example (adjust interface and subnet to your environment):

```bash
docker network create -d macvlan \
  --subnet=192.168.1.0/24 \
  --gateway=192.168.1.1 \
  -o parent=eth0 \
  lan_macvlan
```

### 2. Create the .env file

Create a file named `.env` next to `docker-compose.yaml`.

This file is required and exlcuded via gitignore.

```
###############################################################################
# Pi-hole & unbound – environment configuration
#
# This file is read by Docker Compose via ${VAR} expansion.
# It MUST live in the same directory as docker-compose.yaml.
# Do NOT commit this file to git (it contains secrets).
###############################################################################

### ---------------------------------------------------------------------------
### Networking
### ---------------------------------------------------------------------------

# Static IPv4 address assigned to the container on the macvlan network.
# This IP must:be within the macvlan subnet
PIHOLE_IP=192.168.1.69
UNBOUND_IP=192.168.1.42

### ---------------------------------------------------------------------------
### Timezone
### ---------------------------------------------------------------------------

# Timezone used by the container (affects log timestamps).
# List: https://en.wikipedia.org/wiki/List_of_tz_database_time_zones
PIHOLE_TZ=Europe/London

### ---------------------------------------------------------------------------
### Secrets
### ---------------------------------------------------------------------------

# Pi-hole admin password.
# Use a long, random password.
PIHOLE_PW=change-me-use-a-long-random-password

```

### 3. Configure unbound

Edit `unbound_gfc/unbound.conf` to your liking.

### 4. Start the service

```bash
docker compose up -d
```

Check status:

```bash
docker compose ps
docker compose logs pihole
docker compose logs unbound
```
---

## Accessing Pi-hole

From another machine on the LAN:

http://<PIHOLE_IP>/admin

---

## Credits

## Credits

**Pi-hole**  
Developed by the Pi-hole team  
Project website: https://pi-hole.net  
Source code: https://github.com/pi-hole/pi-hole  

**Unbound**  
Developed by NLnet Labs  
Project website: https://nlnetlabs.nl/projects/unbound/  
Source code: https://github.com/NLnetLabs/unbound
