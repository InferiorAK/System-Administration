## <center> Day 22: Clone Git Repository on Storage Server

```
The DevOps team established a new Git repository last week, which remains unused at present. However, the Nautilus application development team now requires a copy of this repository on the Storage Server in the Stratos DC. Follow the provided details to clone the repository:


The repository to be cloned is located at /opt/beta.git

Clone this Git repository to the /usr/src/kodekloudrepos directory. Perform this task using the natasha user, and ensure that no modifications are made to the repository or existing directories, such as changing permissions or making unauthorized alterations.
```

---

- Login to Storage Server:
    ```apache
    ssh natasha@ststor01
    ```
    Bl@kW
- Create dir:
    ```apache
    sudo mkdir -p /usr/src/kodekloudrepos/beta
    ```
- Fix permission same as the repo:
    ```apache
    sudo chown natasha:natasha /usr/src/kodekloudrepos/beta
    ls -l /usr/src/kodekloudrepos
    ```
- Now, Clone the given repo:
    ```apache
    cd /usr/src/kodekloudrepos/beta
    git clone /opt/beta.git
    ```
    It's safe to use, no mistake will occur then.  
    Or,
    ```apache
    git clone /opt/beta.git /usr/src/kodekloudrepos/beta
    ```