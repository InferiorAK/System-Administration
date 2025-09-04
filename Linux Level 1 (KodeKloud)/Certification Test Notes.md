## <center> Certification Test

### <center> Task - 01

```
1 / 10
Weight: 14
One user needs to be created on an app server in Stratos Datacenter. The system admins team has shared more details below regarding the same.


a. The user name should be mohammed on App server 3 in Stratos Datacenter.

b. Set its UID to 1842 and home directory to /opt/mohammed (you can create this directory if it doesn't exist).

c. Also create a group named nautilus_admin_users on App server 3 in Stratos Datacenter. Add the user mohammed to the nautilus_admin_users group.
```

- Login into App Server 3:
    ```apache
    ssh banner@stapp03
    ```
    BigGr33n
- Create group:
    ```apache
    groupadd nautilus_admin_users
    ```
- Create User:
    ```apache
    useradd -m -d /opt/mohammed -u 1842 mohammed
    ```
- Add to the group:
    ```apache
    usermod -aG nautilus_admin_users mohammed
    ```
- Verify:
    ```apache
    id mohammed
    ```

---


### <center> Task - 02

```
2 / 10
Weight: 8
The system admins team want to create a user on an app server in Stratos Datacenter. Create the user as per the details given below:

Create a user named eric with a non-interactive shell on App server 3 in Stratos Datacenter.
```

- Create the user:
    ```apache
    useradd -s /usr/sbin/nologin eric
    ```
- Verify:
    ```apache
    getent passwd eric
    ```

---


### <center> Task - 03

```
3 / 10
Weight: 4
To ensure compliance with security standards, the Nautilus project team is implementing restrictions on crontab access, specifically dictating which users are permitted to create or modify cron jobs. The assignment is to limit crontab access on App Server 3 according to the following criteria:

Grant crontab access to the gem user while denying access to the kodekloud_cap user.
```

- Restrict access to crontab:
    ```sh
    echo gem > /etc/cron.allow
    echo kodekloud_cap > /etc/cron.deny
    ```
- Verify:
    ```apache
    sudo -u gem crontab -e
    sudo -u kodekloud_cap crontab -e
    ```

---


### <center> Task - 04

```
4 / 10
Weight: 8
The application development team needs some directories created on one of the app servers in Stratos Datacenter. They will use these directories to store some data. They have shared below requirements with us:


Create some directories as below under /opt directory on App server 3 in Stratos Datacenter.

/opt/app/backup/latest
```

- Create dirs:
    ```apache
    mkdir /opt/app
    mkdir /opt/app/backup
    mkdir /opt/app/backup/latest
    ```
- Verify:
    ```apache
    ls -la /opt/app/backup/latest
    ```

---


### <center> Task - 05

```
5 / 10
Weight: 12
The Nautilus security team performed an audit on all servers present in Stratos DC. During the audit some critical data/files were identified which were having the wrong permissions as per security standards. Once the report was shared with the production support team, they started fixing the issues one by one. It has been identified that one of the files named /etc/hosts on Nautilus App 3 server has wrong permissions, so that needs to be fixed and the correct ACLs needs to be applied.

a. User virat must not have any permission on this file.

b. User vivek should have read only permission on this file. Further, sysops group should have read/write permissions on this file.
```

- No permissions to virat:
    ```apache
    setfacl -m u:virat:0 /etc/hosts
    ```
    Or,
    ```apache
    setfacl -x u:virat /etc/hosts
    ```
- read only permission to vivek:
    ```apache
    setfacl -m u:vivek:r /etc/hosts
    ```
- read/write permissions to sysops groups:
    ```apache
    setfacl -m g:sysops:rw /etc/hosts
    ```
- Verify:
    ```apache
    getfacl
    ```
    ```
    user::rw- user:virat:---
    user:vivek:r--
    group::r--
    group:sysops:rw-
    mask::rw-
    other::r--
    ```

---


### <center> Task - 06

```
6 / 10
Weight: 14
The development team requires specific logs stored within the Nautilus storage server situated in the Stratos DC. Access the designated location on the server to retrieve the necessary logs. Further, perform below actions:

Create a tar archive named logs.tar (under natasha's home) of /var/log/ directory.
Now, create a compressed tar archive as well named logs.tar.gz (under natasha's home) of /var/log/ directory.
```

- Login to Nautilus storage server:
    ```apache
    ssh natasha@ststor01
    ```
    Bl@kW
- I am already in **/home/natasha**. So let's go to next steps.
- Create tar archive:
    ```apache
    tar -cvf logs.tar /var/log
    ```
- Create tar.gz archive:
    ```apache
    tar -czvf logs.tar.gz /var/log
    ```

---


### <center> Task - 07

```
7 / 10
Weight: 14
There is some data on Nautilus App Server 3 in Stratos DC. Data needs to be altered in some of the files. On Nautilus App Server 3, alter the /home/BSD.txt file as per details given below.


a. Delete all lines containing the word code and save the results in /home/BSD_DELETE.txt file. (Please be aware of case sensitivity)

b. Replace all occurrences of the word or (look for the exact match) with them and save the results in /home/BSD_REPLACE.txt file.

Note: Let's say you are asked to replace the word to with from. In that case, make sure not to alter any words containing the string itself, for example; upto, contributor etc.
```

- Log into App Server 3:
    ```apache
    ssh banner@stapp03
    ```
    BigGr33n
- Create a backup for safety:
    ```apache
    cp /home/BSD.txt /home/BSD.txt.bak
    ```
- Find the lines containing the word 'code' (Optional, just for a look):
    ```apache
    cat /home/BSD.txt | nl | grep code
    ```
- Delete those lines:
    ```apache
    sed '/code/ d' /home/BSD.txt > /home/BSD_DELETE.txt
    cat /home/BSD_DELETE.txt | grep code
    ```
    - `d` : To delete the matched word
- Check for lines containing **or** (Optional, just for a look):
    ```apache
    cat /home/BSD.txt | grep -wn or
    ```
    - `-w`: Whole Word match
    - `-n`: Line Numbers
- Replace or with from:
    ```apache
    sed 's/or/them/g' /home/BSD.txt > /home/BSD_REPLACE.txt
    ```
- Verify (Optional, just for a look):
    ```apache
    cat /home/BSD.txt | grep -nw or
    cat /home/BSD.txt | grep -nw or | wc -l
    cat /home/BSD_REPLACE.txt | grep -nw them | wc -l
    ```

---


### <center> Task - 08

```
8 / 10
Weight: 12
On our Storage server in Stratos Datacenter we are having some issues where the nfsuser user is holding hundreds of processes, which is degrading the performance of the server. Therefore, we have a requirement to limit its maximum processes. Please set its maximum process limits as below:


a. soft limit = 1024

b. hard_limit = 2024
```

- Login to Nautilus storage server:
    ```apache
    ssh natasha@ststor01
    ```
    Bl@kW
- Check limits:
    ```apache
    ulimit -a
    cat /proc/$$/limits
    ```
- Edit limits configuration:
    ```apache
    vi /etc/security/limits.conf
    ```
    ```yml
    nfsuser  soft    nproc   1024
    nfsuser  hard    nproc   2024
    ```
- Verify changes:
    ```apache
    sudo -u nfsuser ulimit -Su
    sudo -u nfsuser ulimit -Hu
    ```
    *Or, just check using logged in*

---


### <center> Task - 09

```
9 / 10
Weight: 8
The xFusionCorp Industries security team recently did a security audit of their infrastructure and came up with some ideas to improve the application and server security. They decided to use SElinux for an additional security layer. They are still planning how they will implement it, however, they have decided to start testing with app servers, so based on the recommendations they have the following requirements:


Install the required packages of SElinux on App server 3 in Stratos Datacenter and disable it permanently for now, it will be enabled after making some required configuration changes on this host. Don't worry about rebooting the server as there is already a reboot scheduled during tonight's maintenance window. Also ignore the status of the SElinux command line right now; the final status after reboot should be disabled.
```

- Login to App Server 3
- Install all packages:
    ```apache
    yum install -y selinux-policy selinux-policy-targeted policycoreutils
    ```
- Disable SELinux permanently:
    ```apache
    vi /etc/selinux/config
    ```
    ```
    SELINUX=disabled
    ```

---


### <center> Task - 10

```
10 / 10
Weight: 6
The Nautilus system admins team recently deployed a web UI application for their backup utility running on the Nautilus backup server in Stratos Datacenter. The application is running on port 6100. They have a firewalld installed on that server. The requirements that have come up include the following:


Open all incoming connections on 6100/tcp port, zone should be public.
```

- Login to Nautilus backup server:
    ```apache
    ssh clint@stbkp01
    ```
    H@wk3y3
- Set zone to public:
    ```apache
    firewall-cmd --set-default-zone public
    ```
- Open all incoming connections on 6100/tcp port:
    ```apache
    firewall-cmd --zone=public --add-port 6100/tcp --permanent
    ```
- Relod it and Verify:
    ```apache
    firewall-cmd --reload
    firewall-cmd --list-all
    ```

---