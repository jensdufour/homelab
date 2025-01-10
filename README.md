# Zion: Private Cloud

## Services

- [Cloudflared](https://github.com/jensdufour/homelab/tree/main/services/1-cloudflared.md)

## Applications
- [Proxmox](https://github.com/jensdufour/homelab/tree/main/applications/1-proxmox.md)
- [Home Assistant](https://github.com/jensdufour/homelab/tree/main/applications/3-homeassistant.md)
- [Storage](https://github.com/jensdufour/homelab/tree/main/applications/2-storage.md)
- [Docker](https://github.com/jensdufour/homelab/tree/main/applications/4-docker.md)

## Data Directory
General overview of the directory structure in my vault. 
```
data
├── books
├── downloads
│   ├── qbittorrent
│   │   ├── completed
│   │   ├── incomplete
│   │   └── torrents
│   └── nzbget
│       ├── completed
│       ├── intermediate
│       ├── nzb
│       ├── queue
│       └── tmp
├── movies
├── music
└── shows
```

## User Permissions
Using bind mounts can lead to permissions conflicts between the host operating system and the container. To avoid this problem, you we specify the user ID (PUID) and group ID (PGID) in our Docker Compose.

If you run into errors, after creating all the folders you can assign the permissions using chown.
```
sudo chown -R 1000:1000 /data
```

## Setup

1. Proxmox
Proxmox is a powerful open-source server virtualization management solution. Setup instructions can be found [here](https://github.com/jensdufour/homelab/tree/main/applications/1-proxmox.md).

2. Cloudflared: Cloudflared is used to expose your internal services to the internet securely. You can find the setup instructions [here](https://github.com/jensdufour/homelab/tree/main/services/1-cloudflared.md).

3. Backup: Setup a Cron backup script for the Docker containter on your NAS. Setup instructions can be found [here](https://github.com/jensdufour/homelab/tree/main/services/2-backup.md).

4. Home Assistant: Home Assistant is an open-source home automation platform. Setup instructions can be found [here](https://github.com/jensdufour/homelab/tree/main/applications/3-homeassistant.md).

5. Docker: Plex Stack is used for managing and streaming your media. Servarr Stack is a collection of applications to automate media management Setup instructions can be found [here](https://github.com/jensdufour/homelab/tree/main/applications/4-docker.md).