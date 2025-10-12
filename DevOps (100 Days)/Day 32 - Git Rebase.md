## <center> Day 32: Git Rebase

```
The Nautilus application development team has been working on a project repository /opt/news.git. This repo is cloned at /usr/src/kodekloudrepos on storage server in Stratos DC. They recently shared the following requirements with DevOps team:


One of the developers is working on feature branch and their work is still in progress, however there are some changes which have been pushed into the master branch, the developer now wants to rebase the feature branch with the master branch without loosing any data from the feature branch, also they don't want to add any merge commit by simply merging the master branch into the feature branch. Accomplish this task as per requirements mentioned.

Also remember to push your changes once done.
```

---

- Login to Storage Server:
    ```apache
    ssh natasha@ststor01
    ```
    Bl@kW
- Go to repo:
    ```apache
    sudo su
    cd /usr/src/kodekloudrepos/news
    ```
- Go to feature branch and update:
    ```apache
    git checkout feature
    git fetch origin
    ```
- Check logs for the chages:
    ```apache
    git log -p
    ```
- Now rebase with master branch:
    ```apache
    git rebase master
    ```
- Then push changes to origin:
    ```apache
    git push origin feature
    ```
    - Local feature branch history is now different from the remote feature branch (after the rebase).
    - Git is refusing to push, because it doesn't want to overwrite commits on the remote branch.
    - That's expected — because rebasing rewrites commit history, so Git requires a force push.
- In case of solving this:
    ```apache
    git push origin feature --force-with-lease
    ```
