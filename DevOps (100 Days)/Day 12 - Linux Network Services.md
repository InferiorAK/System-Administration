## <center> Day 12: Linux Network Services

```
Our monitoring tool has reported an issue in Stratos Datacenter. One of our app servers has an issue, as its Apache service is not reachable on port 6300 (which is the Apache port). The service itself could be down, the firewall could be at fault, or something else could be causing the issue.


Use tools like telnet, netstat, etc. to find and fix the issue. Also make sure Apache is reachable from the jump host without compromising any security settings.

Once fixed, you can test the same using command curl http://stapp01:6300 command from jump host.
```

---

- Connect to ssh server:
    ```apache
    ssh tony@stapp01
    ```
    pass: Ir0nM@n
- See the Network Statistics:
    ```apache
    netstat -tulnp
    ```
    Output:
    ```
    [root@stapp01 tony]# netstat -tulnp
    Active Internet connections (only servers)
    Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
    tcp        0      0 127.0.0.1:6300          0.0.0.0:*               LISTEN      435/sendmail: accep 
    tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      307/sshd            
    tcp        0      0 127.0.0.11:42057        0.0.0.0:*               LISTEN      -                   
    tcp6       0      0 :::22                   :::*                    LISTEN      307/sshd            
    udp        0      0 127.0.0.11:49997        0.0.0.0:*                           -
    ```
- Here, sendmail server is running on that port, so we have to stop it:
    ```apache
    systemctl stop sendmail
    systemctl status sendmail
    ```
<!-- - Install Apache Server:
    ```apache
    
    ``` -->
- As told, let's start the Apache (httpd) server:
    ```apache
    systemctl start httpd
    systemctl enable httpd
    systemctl status httpd
    ```
- Now Verify again:
    ```apache
    netstat -tulnp | grep 6300
    ```
- Edit the config file `/etc/httpd/conf/httpd.conf` (if needed to change port):
    ```apache
    sudo vi /etc/httpd/conf/httpd.conf
    ```
    ```yml
    Listen 6300
    ```
    Save and exit with `:wq`
- Then restart the server and verify again that this port is working or not:
    ```apache
    systemctl restart httpd
    netstat -tulnp | grep 6300
    ```
- Now see for blocked or any restricted Network Rules:
    ```apache
    iptables -L
    ```
    ```
    Chain INPUT (policy ACCEPT)
    target     prot opt source               destination         
    ...
    REJECT     all  --  anywhere             anywhere             reject-with icmp-host-prohibited
    ...
    ```
- Add a rule at the top of the **INPUT** chain to allow traffic on TCP **6300**:
    ```apache
    iptables -I INPUT -p tcp --dport 6300 -j ACCEPT
    ```
- Now login to the jump host server:
    ```apache
    ssh thor@jump_host
    ```
    pass: mjolnir123s
- Then verify that the site is working or not:
    ```apache
    curl http://stapp01:6300
    ```
    And it worked Perfectly!