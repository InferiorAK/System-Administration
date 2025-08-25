## <center> Day 3: Secure Root SSH Access

- Login:
    ```apache
    ssh tony@IP
    ```
- root mode:
    ```apache
    sudo su
    vi /etc/ssh/sshd_config
    ```
    Make : `PermitRootLogin no`  
    and Save with `Esc :wq`
- Restart ssh server:
    ```apache
    systemctl restart sshd
    ```