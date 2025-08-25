## <center> Day 1: Linux User Setup with Non-Interactive Shell

### My Solving Approach

- Login to the Server that is mentioned with ssh:
    ```apache
    ssh tony@172.16.238.10
    ```
    pass: Ir0nM@n
- Create the give User with **Non-Interactive** shell:
    ```apache
    sudo useradd -s /sbin/nologin javed
    ```
- Verifying the User:
    ```apache
    cat /etc/passwd | grep -i javed
    ```