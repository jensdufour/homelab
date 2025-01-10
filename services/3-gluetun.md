# Gluetun Proxmox
If you encounter the error "Cannot Unix Open TUN device file: operation not permitted and cannot create TUN device file node: operation not permitted" while running this on LXC containers, follow these steps:

1. Find your `servarr` container number. 

2. Edit the LXC configuration file:
    ```bash
    nano /etc/pve/lxc/`<servarr_container_id>`.conf
    ```

3. Add the following lines to the configuration file:
    ```
    net0:...
    lxc.cgroup2.devices.allow: c 10:200 rwm
    lxc.mount.entry: /dev/net dev/net none bind,create=dir
    lxc.mount.entry: /dev/net/tun dev/net/tun none bind,create=file
    onboot:...
    ```

These steps will allow the container to access the TUN device properly.