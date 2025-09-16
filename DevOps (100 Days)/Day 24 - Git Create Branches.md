## <center> Day 24: Git Create Branches

```
Nautilus developers are actively working on one of the project repositories, /usr/src/kodekloudrepos/official. Recently, they decided to implement some new features in the application, and they want to maintain those new changes in a separate branch. Below are the requirements that have been shared with the DevOps team:


On Storage server in Stratos DC create a new branch xfusioncorp_official from master branch in /usr/src/kodekloudrepos/official git repo.

Please do not try to make any changes in the code.
```

---

- Login to Storage Server:
    ```apache
    ssh natasha@ststor01
    ```
    Bl@kW
- Go to **/usr/src/kodekloudrepos/official**
- List all branches:
    ```apache
    git branch -a
    ```
- Switch to master branch:
    ```apache
    git switch master
    ```
- Create branch:
    ```apache
    git branch xfusioncorp_official
    ```