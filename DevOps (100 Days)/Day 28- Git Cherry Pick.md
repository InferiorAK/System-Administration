## <center> Day 28: Git Cherry Pick

```
The Nautilus application development team has been working on a project repository /opt/apps.git. This repo is cloned at /usr/src/kodekloudrepos on storage server in Stratos DC. They recently shared the following requirements with the DevOps team:

There are two branches in this repository, master and feature. One of the developers is working on the feature branch and their work is still in progress, however they want to merge one of the commits from the feature branch to the master branch, the message for the commit that needs to be merged into master is Update info.txt. Accomplish this task for them, also remember to push your changes eventually.
```

---

- Login to Storage Server:
    ```apache
    ssh natasha@ststor01
    ```
    Bl@kW
- Go to the cloned repo's path:
    ```apache
    cd /usr/src/kodekloudrepos
    cd apps
    git config --global --add safe.directory /usr/src/kodekloudrepos/apps
    ```
- Check Branch and Commit logs there:
    ```apache
    git branch -a
    git log --oneline
    ```
    In my case, I was in **feature** branch.
    ```
    cac9d35 (HEAD -> feature, origin/feature) Update welcome.txt
    9a16793 Update info.txt
    c815bd2 (origin/master, master) Add welcome.txt
    74eb97c initial commit
    ```
    Our desired Hash: **9a16793**
- Now switch branch to **master**:
    ```apache
    sudo git checkout master
    ```
- Update with cherry pick and check logs:
    ```apache
    sudo git cherry-pick 9a16793
    git log --oneline
    ```
- Now push to remote:
    ```apache
    sudo git push origin master
    ```

> ***Note**: Working in sude mode is better fot this Challenge*