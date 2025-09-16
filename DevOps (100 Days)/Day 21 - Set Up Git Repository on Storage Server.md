## <center> Day 21: Set Up Git Repository on Storage Server

```
The Nautilus development team has provided requirements to the DevOps team for a new application development project, specifically requesting the establishment of a Git repository. Follow the instructions below to create the Git repository on the Storage server in the Stratos DC:


Utilize yum to install the git package on the Storage Server.

Create a bare repository named /opt/news.git (ensure exact name usage).
```

---

- Login to Storage Server:
    ```apache
    ssh natasha@ststor01
    ```
    Bl@kW
- Install git:
    ```apache
    yum install -y git
    ```
- Create a bare repo:
    ```apache
    git init --bare /opt/news.git
    ```
    - Creates **/opt/news.git/**
    - Inside: only Git internals (HEAD, objects/, refs/, etc.)
    - No working tree (you cannot directly edit project files here).
    - Correct way to create a remote/central repo.