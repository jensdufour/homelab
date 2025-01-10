# Cloudflared

This script sets up Cloudflared on a Proxmox Virtual Environment (VE) container. Cloudflared is a tunneling daemon that proxies traffic from Cloudflare to your local server.

## Usage

To use the Proxmox helper script for setting up Cloudflared, execute the following command in your Proxmox VE environment:
```
bash -c "$(wget -qLO - https://github.com/community-scripts/ProxmoxVE/raw/main/ct/cloudflared.sh)"
```
Use the advanced configuration for this, mainly to set a static IP and disable IPv6.

## Configuration

To configure a Cloudflare Tunnel from the Zero Trust portal with the Cloudflared service install command, follow these steps:

1. **Login to Cloudflare Zero Trust Dashboard**:
    - Navigate to the [Cloudflare Zero Trust Dashboard](https://dash.teams.cloudflare.com/).
    - Log in with your Cloudflare account.

2. **Create a Tunnel**:
    - Go to the "Access" section and select "Tunnels".
    - Click on "Create a tunnel".
    - Provide a name for your tunnel and click "Save".

3. **Install Cloudflared**:
    - Follow the instructions to install Cloudflared on your server if you haven't already done so.

4. **Run the Cloudflared Service Install Command**:
    - Copy the provided command from the Zero Trust Dashboard.
    - Run the command on your Proxmox VE container to install and configure the Cloudflared service. The command will look similar to this:
      ```
      cloudflared service install <TOKEN>
      ```
    - Replace `<TOKEN>` with the actual token provided in the Zero Trust Dashboard.

5. **Verify the Tunnel**:
    - After running the command, verify that the tunnel is active in the Zero Trust Dashboard.
    - You should see your tunnel listed with a status of "Active".

6. **Configure Public Hostname**:
    - Go to the "Public Hostname" section in the Zero Trust Dashboard.
    - Create a public hostname record that points to your service.

Your Cloudflare Tunnel is now configured and ready to use.