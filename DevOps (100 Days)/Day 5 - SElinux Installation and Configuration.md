## <center> Day 5: SElinux Installation and Configuration

```
Following a security audit, the xFusionCorp Industries security team has opted to enhance application and server security with SELinux. To initiate testing, the following requirements have been established for App server 3 in the Stratos Datacenter:



Install the required SELinux packages.

Permanently disable SELinux for the time being; it will be re-enabled after necessary configuration changes.

No need to reboot the server, as a scheduled maintenance reboot is already planned for tonight.

Disregard the current status of SELinux via the command line; the final status after the reboot should be disabled.
```

### Solution

### 1. Install Required SELinux Packages

For RHEL/CentOS/Fedora:
```apache
sudo yum install selinux-policy selinux-policy-targeted policycoreutils
```

For Ubuntu/Debian (only if needed, since they use AppArmor by default):
```apache
sudo apt update
sudo apt install selinux-basics selinux-policy-default auditd
```

---

### 2. Permanently Disable SELinux
Edit the SELinux configuration file:
```apache
sudo nano /etc/selinux/config
```

Change to:
```apache
SELINUX=disabled
```
Save and exit.

---

### 3. No Reboot Required Now
- You do not need to reboot immediately.
- The change will take effect after tonight’s scheduled maintenance reboot.

---

### 4. Important Notes
- The current output of `getenforce` or `sestatus` will still show **Enforcing** or **Permissive** until the reboot.
- After the reboot, `sestatus` should display: