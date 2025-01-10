### nzbGet  - Categories fix
To fix the "directory does not appear to exist inside the container" error in Sonarr and Radarr, follow these steps:

**NZBGet Configuration:**
- Once NZBGet is set up, go to settings.
- Under INCOMING NZBS, change the `AppendCategoryDir` to `No`. This will prevent some potential mapping issues and save on unnecessary directories.