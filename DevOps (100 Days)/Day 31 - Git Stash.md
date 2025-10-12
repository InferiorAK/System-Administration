## <center> Day 31: Git Stash

git stash is used to temporarily save (or “stash”) your uncommitted changes in a Git repository without committing them — so you can work on something else and come back later to restore them.

```
The Nautilus application development team was working on a git repository /usr/src/kodekloudrepos/media present on Storage server in Stratos DC. One of the developers stashed some in-progress changes in this repository, but now they want to restore some of the stashed changes. Find below more details to accomplish this task:


Look for the stashed changes under /usr/src/kodekloudrepos/media git repository, and restore the stash with stash@{1} identifier. Further, commit and push your changes to the origin.
```

---

- Login to Storage Server:
    ```apache
    ssh natasha@ststor01
    ```
    Bl@kW
- Go to the repo:
    ```apache
    cd /usr/src/kodekloudrepos/media
    sudo su
    ```
- List stashes:
    ```apache
    git stash list
    ```
- Restores the stashed changes from `stash@{1}`:
    ```apache
    git stash apply stash@{1}
    ```
    Then I got welcome.txt restored.
- Now add all to stage, commit:
    ```apache
    git add .
    git commit -m "Restored stash@{1}"
    ```
- Then push changes to origin:
    ```apache
    git remote -v
    git push origin
    ```
