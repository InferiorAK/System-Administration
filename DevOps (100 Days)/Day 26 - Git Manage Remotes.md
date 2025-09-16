## <center> Day 26: Git Manage Remotes

```
The xFusionCorp development team added updates to the project that is maintained under /opt/demo.git repo and cloned under /usr/src/kodekloudrepos/demo. Recently some changes were made on Git server that is hosted on Storage server in Stratos DC. The DevOps team added some new Git remotes, so we need to update remote on /usr/src/kodekloudrepos/demo repository as per details mentioned below:

a. In /usr/src/kodekloudrepos/demo repo add a new remote dev_demo and point it to /opt/xfusioncorp_demo.git repository.

b. There is a file /tmp/index.html on same server; copy this file to the repo and add/commit to master branch.

c. Finally push master branch to this new remote origin.
```

---

- Login to Storage Server:
    ```apache
    ssh natasha@ststor01
    ```
    Bl@kW
- Go to the repo:
    ```apache
    cd /usr/src/kodekloudrepos/demo
    ```
- Add a new remote **dev_demo** pointing to **/opt/xfusioncorp_demo.git**:
    ```apache
    git remote add dev_demo /opt/xfusioncorp_demo.git
    ```
- Verify it:
    ```apache
    git remote -v
    ```
- Copy the file into the repo:
    ```apache
    cp /tmp/index.html .
    ```
- Stage and commit the file on master branch:
    ```apache
    git add index.html
    git commit -m "Add index.html from storage server"
    ```
- Push master branch to the new remote dev_demo:
    ```apache
    git push dev_demo master
    ```
