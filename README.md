# WireGuard and Pi-hole VPN Project

[![Made With - WireGuard](https://img.shields.io/badge/Made%20With-WireGuard-0077b6?logo=wireguard&logoColor=white&style=flat-square)](https://www.wireguard.com/)
[![Dynamic DNS - DuckDNS](https://img.shields.io/badge/Dynamic%20DNS-DuckDNS-5c6bc0?logo=duckduckgo&logoColor=white&style=flat-square)](https://www.duckdns.org/)
[![Dockerized](https://img.shields.io/badge/Dockerized-Yes-0db7ed?logo=docker&logoColor=white&style=flat-square)](https://www.docker.com/)

## About This Project

This project sets up a lightweight WireGuard VPN and Pi-hole DNS server on a Raspberry Pi using Docker Compose.  
It supports DNS-only or full VPN tunneling for remote ad-blocking, secure DNS lookups, and private internet access.  
Dynamic DNS updates are handled automatically through DuckDNS.

All secrets and private keys are externalized into a protected folder to make the repository safe for public sharing.

## Features

- WireGuard VPN server
- Centralized Pi-hole DNS ad-blocking
- DNS-only tunneling or full traffic tunneling
- Lightweight Docker deployment optimized for Raspberry Pi
- Dynamic DNS support using DuckDNS
- Secrets externalized for safe public GitHub sharing

## Stack

| Technology | Purpose |
|:--|:--|
| WireGuard | VPN server |
| Pi-hole | Ad-blocking DNS server |
| DuckDNS | Dynamic DNS updates |
| Docker and Docker Compose | Container management |
| Raspberry Pi | Hardware platform |

## Setup Instructions

1. Clone this repository.
2. Copy the example files to real ones:
   ```bash
   cp docker-compose-example.yml docker-compose.yml
   cp duckdns/docker-compose-example.yml duckdns/docker-compose.yml
   ```
3. Create a `secrets/` folder at the root of the project.

4. Inside `secrets/`, create the following files:
   - `.env-wireguard`
   - `.env-duckdns`
   - `iphone.conf`
   - `macbookair.conf`

5. Fill in your real secrets and private keys. See examples below.

6. Start the services using Docker Compose:
   ```bash
   docker-compose up -d
   ```

## Creating the Secrets Folder

The `secrets/` folder contains all private environment files and WireGuard client configurations.  
It is protected by `.gitignore` and must not be committed to any public repository.

### Required files inside `secrets/`

| File | Purpose |
|:--|:--|
| `.env-wireguard` | WireGuard container environment variables |
| `.env-duckdns` | DuckDNS container environment variables |
| `iphone.conf` | WireGuard client configuration for iPhone |
| `macbookair.conf` | WireGuard client configuration for MacBook |

### Example .env-wireguard

```plaintext
PUID=1000
PGID=1000
TZ=America/Chicago
SERVERURL=your-subdomain.duckdns.org
SERVERPORT=51820
PEERS=2
PEERDNS=10.0.0.1
INTERNAL_SUBNET=10.0.0.0/24
```

### Example .env-duckdns

```plaintext
PUID=1000
PGID=1000
TZ=America/Chicago
SUBDOMAINS=your-subdomain
TOKEN=your-duckdns-token
LOG_FILE=false
```

## Generating WireGuard Keys

If you want to create your own client keys instead of using pre-generated ones:

1. SSH into your Raspberry Pi or local machine.
2. Run the following to create a private and public key pair:

```bash
wg genkey | tee private.key | wg pubkey > public.key
```

This generates:
- `private.key`
- `public.key`

Use the contents of these files in your client `.conf` files.

## Example Client Configuration

### Example iphone.conf

```ini
[Interface]
PrivateKey = your_client_private_key_here
Address = 10.0.0.2/32
DNS = 10.0.0.1

[Peer]
PublicKey = your_server_public_key_here
Endpoint = your-subdomain.duckdns.org:51820
AllowedIPs = 10.0.0.1/32
PersistentKeepalive = 25
```

### Example macbookair.conf

```ini
[Interface]
PrivateKey = your_client_private_key_here
Address = 10.0.0.3/32
DNS = 10.0.0.1

[Peer]
PublicKey = your_server_public_key_here
Endpoint = your-subdomain.duckdns.org:51820
AllowedIPs = 10.0.0.1/32
PersistentKeepalive = 25
```

## Folder Structure

```plaintext
wireguard-vpn-project/
├── docker-compose-example.yml
├── duckdns/
│   └── docker-compose-example.yml
├── configs/
│   ├── iphone-example.conf
│   ├── macbookair-example.conf
├── secrets/   (local only, not committed)
│   ├── .env-wireguard
│   ├── .env-duckdns
│   ├── iphone.conf
│   ├── macbookair.conf
├── README.md
├── .gitignore
```

## Security Notes

- Never commit the `secrets/` folder or any real `.env` files
- All secrets are protected by `.gitignore`
- Real private keys must remain local and private

## Project Status

This project is fully functional, safe for public sharing, and ready for further customization.  
You can easily expand it by adding more clients or integrating additional security features as needed.
