## <center> Day 13: IPtables Installation And Configuration

```
We have one of our websites up and running on our Nautilus infrastructure in Stratos DC. Our security team has raised a concern that right now Apache’s port i.e 5003 is open for all since there is no firewall installed on these hosts. So we have decided to add some security layer for these hosts and after discussions and recommendations we have come up with the following requirements:


1. Install iptables and all its dependencies on each app host.

2. Block incoming port 5003 on all apps for everyone except for LBR host.

3. Make sure the rules remain, even after system reboot.
```

---

- Login to the server:
    ```apache
    ssh tony@stapp01
    ssh steve@stapp01
    ssh banner@stapp01
    ```
    Ir0nM@n  
    Am3ric@  
    BigGr33n
- Install IPtables:
    ```apache
    yum install -y iptables iptables-services
    ```
-  Block All on 8087 except for LBR host:
    ```apache
    iptables -A INPUT -s stlb01 -p tcp --dport 5003 -j ACCEPT
    iptables -A INPUT -p tcp --dport 5003 -j DROP
    ```
    > 1st one only accepts from stlb01.  
    > 2nd one drops all from others.
- To verify it's added check here:
    ```apache
    iptables -L -n -v
    ```
- Persistence across reboot:
    ```apache
    # saves current rules into /etc/sysconfig/iptables
    service iptables save

    # enable iptables service
    systemctl enable iptables

    # start iptables service
    systemctl start iptables

    # Check status
    systemctl status iptables
    ```
    Thus do for all 3 servers.

---


### Oneshot Script

- First of all, generate ssh keypairs and upload it to the servers:
    ```apache
    ssh-keygen -t rsa -b 2048
    for host in stapp01 stapp02 stapp03; do
        ssh-copy-id $host
    done
    ```
- Save this script and run it:
    ```apache
    #!/bin/bash

    ssh -t tony@stapp01 "sudo bash -c '
    yum install -y iptables iptables-services
    iptables -A INPUT -s stlb01 -p tcp --dport 5003 -j ACCEPT
    iptables -A INPUT -p tcp --dport 5003 -j DROP
    iptables -L -n
    service iptables save
    systemctl enable iptables
    systemctl start iptables
    systemctl status iptables'"
    ```
    Now, run this for all servers.