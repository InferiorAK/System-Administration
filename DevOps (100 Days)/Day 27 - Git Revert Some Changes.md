## <center> Day 27: Git Revert Some Changes

```
The Nautilus application development team was working on a git repository /usr/src/kodekloudrepos/apps present on Storage server in Stratos DC. However, they reported an issue with the recent commits being pushed to this repo. They have asked the DevOps team to revert repo HEAD to last commit. Below are more details about the task:

In /usr/src/kodekloudrepos/apps git repository, revert the latest commit ( HEAD ) to the previous commit (JFYI the previous commit hash should be with initial commit message ).

Use revert apps message (please use all small letters for commit message) for the new revert commit.
```

---

- Login to Storage Server:
    ```apache
    ssh natasha@ststor01
    ```
    Bl@kW
- Go to the repo path:
    ```apache
    cd /usr/src/kodekloudrepos/apps
    ```
- Revert the latest commit without `-m`:
    ```apache
    git revert HEAD -n
    ```
    - `git revert HEAD` → creates a new commit that undoes the changes introduced by the latest commit (**HEAD**).
    - `-n` / `--no-commit` → applies the revert but does not create a commit yet.
- Commit the revert with the given msg:
    ```apache
    git commit -m "reverse apps"
    ```
- Verify the revert:
    ```apache
    git log -p
    ```