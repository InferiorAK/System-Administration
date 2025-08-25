## <center> Day 17: Install and Configure PostgreSQL

```
The Nautilus application development team has shared that they are planning to deploy one newly developed application on Nautilus infra in Stratos DC. The application uses PostgreSQL database, so as a pre-requisite we need to set up PostgreSQL database server as per requirements shared below:


PostgreSQL database server is already installed on the Nautilus database server.

a. Create a database user kodekloud_sam and set its password to TmPcZjtRQx.

b. Create a database kodekloud_db3 and grant full permissions to user kodekloud_sam on this database.


Note: Please do not try to restart PostgreSQL server service.
```

---

- Login to server:
    ```apache
    ssh peter@stdb01
    ```
    Sp!dy
- Login to postgres user shell:
    ```apache
    su - postgres
    or,
    sudo -i -u postgres
    ```
- Get into the PostgreSQL prompt:
    ```apache
    psql
    \h  # for command helps
    ```
- Create user with the given pass:
    ```sql
    CREATE USER kodekloud_sam WITH PASSWORD 'TmPcZjtRQxs';
    ```
    Verify user:
    ```apache
    \du
    ```
- Create the database:
    ```sql
    CREATE DATABASE kodekloud_db3;
    ```
- Gran All privileges to the user:
    ```sql
    GRANT ALL PRIVILEGES ON DATABASE kodekloud_db3 TO kodekloud_sam;
    ```
    Verify database:
    ```apache
    \l
    ```
- For quiting:
    ```apache
    \q
    ```
All are done by now!

---

### One Liner Solve

```apache
sudo -i -u postgres psql \
-c "DROP DATABASE IF EXISTS kodekloud_db3;" \
-c "DROP USER IF EXISTS kodekloud_sam;" \
-c "CREATE USER kodekloud_sam WITH PASSWORD 'TmPcZjtRQx';" \
-c "CREATE DATABASE kodekloud_db3;" \
-c "GRANT ALL PRIVILEGES ON DATABASE kodekloud_db3 TO kodekloud_sam;" \
-c "\du" \
-c "\l"
```

---

### Be Cautious

| Quote Type      | Usage                          | Example                          | Notes |
|-----------------|--------------------------------|---------------------------------|-------|
| Single `' '`    | String literal (values)        | `PASSWORD 'mypassword'`         | Correct for passwords/text |
| Double `"` "`"  | Identifiers (tables/columns)   | `CREATE TABLE "User" (id int);` | Not for strings |
| No quotes       | Identifiers only               | `CREATE USER kodekloud_sam;`    | Works only for lowercase, non-reserved names |

---

- Always use **single quotes** for passwords or string values.  
- Using double quotes or no quotes for strings → **syntax error**.
