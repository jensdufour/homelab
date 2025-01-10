# Storage - TODO: Currently you just create one disk image and keep mounting this -> Not the right way to go. 

This script sets up Cockpit on a Proxmox Virtual Environment (PVE) using a bash command. It downloads and executes a script from a GitHub repository to automate the installation process.

## Usage

To set up Home Assistant, run the following command in your Proxmox shell:
```
bash -c "$(wget -qLO - https://github.com/community-scripts/ProxmoxVE/raw/main/ct/cockpit.sh)"
```
Use the advanced configuration for this, mainly to set a static IP and disable IPv6.

## Configuration

### Mount the ZFS pool to Cockpit
First, mount the ZFS directory to the Cockpit LXC using the Proxmox web GUI:
- Navigate to the Proxmox web interface.
- Select the LXC container you want to mount the ZFS directory to.
- Go to the "Resources" tab.
- Click on "Add" and select "Mount Point".
- Set the "Storage" to the ZFS pool and the "Mount Point" to `/data`.
- Click "Add" to save the configuration.

### Install Cockpit Identities, File Sharing and Identities
Run these commands from the Cockpit console in Proxmox Virtual Environment (PVE):
```
wget https://github.com/45Drives/cockpit-identities/releases/download/vX.X.X/cockpit-identities_X.X.X_amd64.deb
dpkg -i cockpit-identities_X.X.X_amd64.deb
wget https://github.com/45Drives/cockpit-file-sharing/releases/download/vX.X.X/cockpit-file-sharing_X.X.X_amd64.deb
dpkg -i cockpit-file-sharing_X.X.X_amd64.deb
wget https://github.com/45Drives/cockpit-navigator/releases/download/vX.X.X/cockpit-navigator_X.X.X_amd64.deb
dpkg -i cockpit-navigator_X.X.X_amd64.deb
```
Replace `X.X.X` with the latest version numbers available on the respective GitHub pages.
Refresh the Cockpit GUI with ctrl+shift+r, the relevant components should now be visible.
After completing these steps, you should have Cockpit set up with the necessary components installed.

### Creating the Folder Structure

To create the folder structure, run this command from the Cockpit console in Proxmox Virtual Environment (PVE):
```
mkdir -p /data/{books,movies,music,shows,downloads/{qbittorrent/{completed,incomplete,torrents},nzbget/{completed,intermediate,nzb,queue,tmp}}}
```

### Configuring Cockpit

To add a group `data-share` that contains a user `media`, follow these steps:

1. **Access the Cockpit Web Interface**

    Open your web browser and navigate to the Cockpit web interface. Log in with your administrative credentials.

2. **Create a New Group**

    - In the "Identities" section, switch to the "Groups" tab.
    - Click on "Create New Group".
    - Enter `data-share` as the group name.
    - Click "Create" to add the new group.

3. **Create a New User**

    - Go to the "Identities" section in the Cockpit interface.
    - Switch to the "Users" tab.
    - Click on "New User".
    - Enter `media` as the username and fill in the required details.
    - Add the group `data-share` created in the previous step to the user.
    - Set a password for the `media` user.
    - Click "Create" to add the new user.
    - Once the user has been created set a Samba password for the `media` user.

After completing these steps, you will have successfully created a `data-share` group and added the `media` user to it using the Cockpit GUI.

### Creating a File Share in Cockpit

To create a share through the File Sharing GUI in Cockpit and configure the global settings, follow these steps:

1. **Access the Cockpit Web Interface**

    Open your web browser and navigate to the Cockpit web interface. Log in with your administrative credentials.

2. **Open the File Sharing Interface**

    - In the Cockpit web interface, navigate to the "File Sharing" section.

3. **Configure Global Settings**

    - Click on the "Global Configuration" tab.
    - Set the "Server Description" to `Vault`.
    - Ensure the "Workgroup" is set to `workgroup`.
    - Enable the "Global macOS Shares" option.
    - Click "Save" to apply the global configuration settings.

4. **Create a New Share**

    - Switch to the "Shares" tab in the File Sharing interface.
    - Click on "Add New Share".
    - Enter `vault` as the share name.
    - Set the "Path" to `/data`.
    - Enable the "Windows ACLs with Linux/MacOS Support" option.
    - Enable the "MacOS Share" option.
    - Click "Create" to add the new share.

After completing these steps, you will have successfully configured the global settings and created a new share named `vault` with the specified options using the Cockpit File Sharing GUI.