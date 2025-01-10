# Home Assistant

This script sets up Home Assistant on a Proxmox Virtual Environment (PVE) using a bash command. It downloads and executes a script from a GitHub repository to automate the installation process.

## Usage

To set up Home Assistant, run the following command in your Proxmox shell:
```
bash -c "$(wget -qLO - https://github.com/community-scripts/ProxmoxVE/raw/main/vm/haos-vm.sh)"
```
Use the advanced configuration for this, mainly to set a static IP and disable IPv6.

## Configuration

 After the installation of Home Assistant, follow these steps to complete the setup and configure remote access:
 
 1. Open your web browser and navigate to `http://homeassistant:8123`. This will take you to the Home Assistant setup page.
 2. Follow the on-screen instructions to complete the initial setup of Home Assistant.
 3. To enable remote access, you need to configure a public hostname using Cloudflare:
    - Log in to your Cloudflare account.
    - Navigate to the Zero Trust dashboard.
    - Go to the configuration of the Cloudflare Tunnel you created.
    - Create a public hostname pointing to your Home Assistant instance.
    - Ensure that the DNS record is proxied (orange cloud icon) to enable Cloudflare's security and performance features.
 4. Update your Home Assistant configuration to use the new public hostname for remote access.
 5. Save the configuration and restart Home Assistant to apply the changes.
 
 You should now be able to access your Home Assistant instance remotely using the configured public hostname.
