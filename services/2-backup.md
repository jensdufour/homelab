# Rclone

## Install rClone on the host

```sh
apt install rclone
```

## Create the Azure Storage account 
To create an Azure Storage account and a blob container called `homelab`, follow these steps:

1. Sign in to the [Azure portal](https://portal.azure.com/).

2. In the left-hand menu, select **Storage accounts** and then click **+ Create**.

3. In the **Basics** tab, enter the following information:
    - **Subscription**: Select your Azure subscription.
    - **Resource group**: Select an existing resource group or create a new one.
    - **Storage account name**: Enter a unique name for your storage account.
    - **Region**: Select the region where you want to create the storage account.
    - **Performance**: Select `Standard`.
    - **Account kind**: Select `StorageV2 (general-purpose v2)`.
    - **Replication**: Select the replication option that suits your needs.

4. Click **Review + create** and then **Create**.

5. Once the storage account is created, navigate to the storage account and select **Containers** under the **Data storage** section.

6. Click **+ Container** to create a new container.

7. Enter `homelab` as the name of the container and set the **Public access level** to `Private (no anonymous access)`.

8. Click **Create** to create the container.

You now have an Azure Storage account with a blob container named `homelab`.

## Configure rclone

### Configure the rclone Remote
```sh
rclone config

# Follow the prompts to create a new remote configuration
# Select "n" for a new remote
# Enter a name for the remote, e.g., "azureblob"
# Select "30" for Azure Blob Storage
# Follow the prompts to enter your Azure Storage account name and key
# Leave the advanced configuration options as default
# Select "y" to confirm the configuration

# Example configuration:
# [azureblob]
# type = azureblob
# account = your_account_name
# key = your_account_key
```

### create the back-up script
1. Replace `<source>` with the path to the directory or file you want to copy.
2. Replace `<remote>` with the name of your rclone remote configuration for Azure Storage.
3. Replace `<container>` with the name of your Azure Storage container.

```sh
#!/bin/bash

# Set the source and destination paths
source_path="<source_path>"
destination_path="<destination_path>"
container="<container>"

# Copy the local folder to Azure Blob Storage
rclone sync -P -L --ignore-errors $source_path $destination_path:$container --exclude "qbittorrent/qBittorrent/ipc-socket"
# Print a timestamp for loggigg purposes
echo "Backup completed at $(date)" >> /root/backup.log
```

This script will recursively copy all files and directories from the source to the specified Azure Storage container.

### Make the script executable
```sh
chmod +x /root/backup.sh
```

### Schedule the backup daily
To schedule the backup script to run daily, you can create a cron job. Follow these steps:

1. Open the crontab file for editing:
    ```sh
    crontab -e
    ```

2. Add the following line to schedule the backup script to run daily at 2 AM:
    ```sh
    0 2 * * * </path/to/backup.sh>
    ```

    Make sure to replace `</path/to/backup.sh>` with the actual path to your backup script.

3. Save and close the crontab file.

This cron job will execute the backup script every day at 2 AM.

## More Information

For more details on rclone and its configuration, visit [rclone.org](https://rclone.org/).