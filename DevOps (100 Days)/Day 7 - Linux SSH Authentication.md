## <center> Day 7: Linux SSH Authentication

```
The system admins team of xFusionCorp Industries has set up some scripts on jump host that run on regular intervals and perform operations on all app servers in Stratos Datacenter. To make these scripts work properly we need to make sure the thor user on jump host has password-less SSH access to all app servers through their respective sudo users (i.e tony for app server 1). Based on the requirements, perform the following:



Set up a password-less authentication from user thor on jump host to all app servers through their respective sudo users.
```

- Make sure you are logged into the thor user on jumphost
- Now generate ssh-key:
    ```apache
    ssh-keygen -t rsa -b 2048
    ```
    Press Enter for all prompts (no passphrase, default path `~/.ssh/id_rsa`)
- No copy the id key to all the servers that we are desired to login again without password in future:
    ```apache
    ssh-copy-id tony@172.16.238.10
    ssh-copy-id steve@172.16.238.11
    ssh-copy-id banner@172.16.238.12
    ```
    pass:
    ```yml
    Ir0nM@n
    Am3ric@
    BigGr33n
    ```
- Now all are done.