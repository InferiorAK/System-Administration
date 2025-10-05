## <center> Day 30: Git hard reset

```
The Nautilus application development team was working on a git repository /usr/src/kodekloudrepos/official present on Storage server in Stratos DC. This was just a test repository and one of the developers just pushed a couple of changes for testing, but now they want to clean this repository along with the commit history/work tree, so they want to point back the HEAD and the branch itself to a commit with message add data.txt file. Find below more details:


In /usr/src/kodekloudrepos/official git repository, reset the git commit history so that there are only two commits in the commit history i.e initial commit and add data.txt file.

Also make sure to push your changes.
```

---

- Login to Storage Server:
    ```apache
    ssh natasha@ststor01
    ```
    Bl@kW
- Go to repo:
    ```apache
    cd /usr/src/kodekloudrepos/official
    ```
- Check log:
    ```apache
    sudo su
    git log
    ```
- Hard reset for all those 10 commits after **initial commit** and add **data.txt file**:
    ```apache
    git reset --hard HEAD~10
    ```
- Now push update to remote repo:
    ```apache
    git branch -a
    git remote -v
    git push origin master
    ```
- Now I am getting rejected issue. To solve it:
    ```apache
    git push origin master --force
    ```
