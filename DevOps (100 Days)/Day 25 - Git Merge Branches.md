## <center> Day 25: Git Merge Branches

```
The Nautilus application development team has been working on a project repository /opt/beta.git. This repo is cloned at /usr/src/kodekloudrepos on storage server in Stratos DC. They recently shared the following requirements with DevOps team:


Create a new branch xfusion in /usr/src/kodekloudrepos/games repo from master and copy the /tmp/index.html file (present on storage server itself) into the repo. Further, add/commit this file in the new branch and merge back that branch into master branch. Finally, push the changes to the origin for both of the branches.
```

---

- Login to Storage Server:
    ```apache
    ssh natasha@ststor01
    ```
    Bl@kW
- Go to **/usr/src/kodekloudrepos/games**
- Create new branch named xfusion:
    ```apache
    git branch xfusion
    ```
- Switch to xfusion:
    ```apache
    git switch xfusion    
    ```
- Copy the file to repo:
    ```apache
    cp /tmp/index.html .
    ```
- Stage the file:
    ```apache
    git add index.html
    ```
- Commit the file:
    ```apache
    git commit -m "index committed" index.html
    ```
- Switch back to master:
    ```apache
    git switch master
    ```
- Merge the new branch into master:
    ```apache
    git merge xfusion
    ```
- Push the changes:
    ```apache
    git push origin master
    git push origin xfusion
    ```