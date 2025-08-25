## <center> Day 10: Linux Bash Scripts

```
The production support team of xFusionCorp Industries is working on developing some bash scripts to automate different day to day tasks. One is to create a bash script for taking websites backup. They have a static website running on App Server 1 in Stratos Datacenter, and they need to create a bash script named official_backup.sh which should accomplish the following tasks. (Also remember to place the script under /scripts directory on App Server 1).


a. Create a zip archive named xfusioncorp_official.zip of /var/www/html/official directory.

b. Save the archive in /backup/ on App Server 1. This is a temporary storage, as backups from this location will be clean on weekly basis. Therefore, we also need to save this backup archive on Nautilus Backup Server.

c. Copy the created archive to Nautilus Backup Server server in /backup/ location.

d. Please make sure script won't ask for password while copying the archive file. Additionally, the respective server user (for example, tony in case of App Server 1) must be able to run it.
```

---


- Connect to the Server:
    ```apache
    ssh tony@172.16.238.10
    ```
    pass: Ir0nM@n
- Creating **official_backup.sh** on **/scripts**:
    ```apache
    #!/bin/bash

    zip -r -9 /backup/xfusioncorp_official.zip /var/www/html/official
    scp /backup/xfusioncorp_official.zip clint@172.16.238.16:/backup
    ```
- Set Permission for the script:
    ```apache
    sudo chmod 777 official_backup.sh
    ```
- Now creating ssh keys and uploading authorized_keys to the backup server:
    ```apache
    ssh-keygen -t rsa -b 4096
    ssh-copy-id clint@172.16.238.16
    ```
    pass: H@wk3y3
    > Note: This must be added as user tony
- Now run the script:
    ```apache
    ./official_backup.sh
    ```