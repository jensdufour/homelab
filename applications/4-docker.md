# Plex Stack

To create an LXC container with specific configurations, follow the steps below:

## Usage
Run the Command to create the Docker container:

    Open your terminal and execute the following command:
    ```bash
    bash -c "$(wget -qLO - https://github.com/community-scripts/ProxmoxVE/raw/main/ct/docker.sh)"
    ```
Use the advanced configuration for both stacks, mainly to set a hostname,static IP, disable IPv6 and give it the required performance.

### Plex stack

- **Hostname:** plex
- **Resources:**
    - **Storage:** 32 GB
    - **vCPU:** 4 vCPUs
    - **RAM:**  8192 MiB 
- **Static IP:** Assign a static IP address to the container.
- **Disable IPv6:** Ensure that IPv6 is disabled for the container.

### Servarr stack

- **Hostname:** servarr
- **Resources:**
    - **Storage:** 4 GB
    - **vCPU:** 4 vCPUs
    - **RAM:**  8192 MiB 
- **Static IP:** Assign a static IP address to the container.
- **Disable IPv6:** Ensure that IPv6 is disabled for the container.

There is no real need to add portainer to any of the stacks, as we will manage them through Docker Compose.

## Configuration

### Configuration Files Storage

Create a mount point in the storage LXC with the path `/docker` and storage as `local-lvm` to leverage the SSD, follow these steps:

1. Open the storage LXC and navigate to the **Resources** section.
2. Select Add Mount Point:
    - **Storage**: `local-lvm`
    - **Size**: `128 GB`
    - **Path**: `/docker`

This setup will store all Docker configurations, which can then be backed up to Azure, as described in the services overview.

### Mount the Configuration Files Storage

To copy the mount points from the Cockpit LXC in `/etc/pve/lxc/<id>.conf` to the Plex and Servarr LXC configuration, follow these steps:


1. **Copy the Mount Points:**
    - Identify and copy the mount point configurations from the Cockpit LXC configuration file. The lines will look similar to:
      ```
      mp0: /path/on/host,mp=/path/in/container
      mp1: /path/on/host,mp=/path/in/container
      ```

2. **Update the Plex/Servarr LXC Configuration:**
    - Open the Plex LXC configuration file:
      ```bash
      nano /etc/pve/lxc/<plex-lxc-id>.conf
      ```
    - Paste the copied mount point configurations into the Plex LXC configuration file.

3. **Save and Exit:**
    - Save the changes and exit the editor.

4. **Restart the Plex LXC Container:**

This will ensure that the Plex LXC container has the same mount points as the Cockpit LXC container.

### Docker Compose
To configure the Plex and Servarr Stack, we will use Docker Compose. The Docker Compose file can be found at the following link: [Plex - Docker Compose File](https://github.com/jensdufour/homelab/tree/main/assets/compose/plex.yaml) and [Servarr - Docker Compose File](https://github.com/jensdufour/homelab/tree/main/assets/compose/servarr.yaml). 
The Docker Compose was made using the Fleet of [LinuxServer.io](https://fleet.linuxserver.io). 
On the Plex stack the configured containers are Plex, Overseer and Tautulli. 

- **Plex:** A media server that organizes video, music, and photos from personal media libraries and streams them to smart TVs, streaming boxes, and mobile devices.
- **Overseer:** A request management and media discovery tool that integrates with Plex to allow users to request new content and track media availability.
- **Tautulli:** A monitoring and tracking tool for Plex Media Server that provides detailed insights into server activity, including user watch history and media consumption statistics.

On the Servarr stack the configured containers are qBittorrent, nzbGet, Prowlarr, Sonarr, Radarr and Bazarr. 

- **Gluetun:** VPN client to obscude the traffic from qBittorrent and nzbGet.
- **qBittorrent:** A free and open-source BitTorrent client.
- **nzbGet:** A binary newsgrabber for downloading from Usenet.
- **Prowlarr:** An indexer manager/proxy for managing both Torrent and Usenet indexers.
- **Sonarr:** A PVR for Usenet and BitTorrent users to manage TV series.
- **Radarr:** A movie collection manager for Usenet and BitTorrent users.
- **Bazarr:** A companion tool to Sonarr and Radarr for managing subtitles.

Follow these steps to set up the Plex/Servarr Stack using Docker Compose:

1. Create the `docker-compose.yaml` file and paste the contents from the link provided.
2. Run the following command to start the Plex Stack:

```bash
docker-compose up -d
```

This will set up and run the Plex Stack with the specified configurations.