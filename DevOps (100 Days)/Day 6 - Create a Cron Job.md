## <center> Day 6: Create a Cron Job

```
The Nautilus system admins team has prepared scripts to automate several day-to-day tasks. They want them to be deployed on all app servers in Stratos DC on a set schedule. Before that they need to test similar functionality with a sample cron job. Therefore, perform the steps below:



a. Install cronie package on all Nautilus app servers and start crond service.


b. Add a cron */5 * * * * echo hello > /tmp/cron_text for root user.
```


- Login to ssh:
    ```apache
    ssh steve@172.16.238.11
    ```
    pass: Am3ric@
- root mode:
    ```apache
    sudo su
    ```
- Install cronie (for crontab):
    ```apache
    yum install cronie -y
    ```
- Start crontab service:
    ```apache
    systemctl start crond
    ```
- Edit the crontab for the current user (as for root):
    ```apache
    crontab -e
    ```
- Now add this line to it:
    ```apache
    */5 * * * * echo hello > /tmp/cron_text
    ```
    Save and exit with `:wq`  
    Explanation:
    - `*/5` — every 5 minutes  
    - `*` (hour) — every hour  
    - `*` (day of month) — every day of the month  
    - `*` (month) — every month  
    - `*` (day of week) — every day of the week  
    - `echo hello` — prints "hello"  
    - `>` — overwrites the file  
    - `/tmp/cron_text` — file where output is saved
- Retart crontab service (If needed):
    ```apache
    systemctl restart crond
    ```