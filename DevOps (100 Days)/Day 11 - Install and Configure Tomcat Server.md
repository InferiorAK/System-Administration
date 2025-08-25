## <center> Day 11: Install and Configure Tomcat Server

```
The Nautilus application development team recently finished the beta version of one of their Java-based applications, which they are planning to deploy on one of the app servers in Stratos DC. After an internal team meeting, they have decided to use the tomcat application server. Based on the requirements mentioned below complete the task:



a. Install tomcat server on App Server 3.

b. Configure it to run on port 8084.

c. There is a ROOT.war file on Jump host at location /tmp.


Deploy it on this tomcat server and make sure the webpage works directly on base URL i.e curl http://stapp03:8084
```

---

### About Tomcat

### Apache Tomcat Overview

- **Type:** Open-source Java Servlet Container / Web Server  
- **Purpose:**  
    Tomcat is used to **run Java-based web applications**, specifically **Java Servlets** and **JavaServer Pages (JSP)**. It provides a **web server environment** for executing Java code and serving dynamic content over HTTP.  

- **Key Features:**  
    - Implements **Java EE (Jakarta EE) specifications** for Servlets and JSP.  
    - Supports **HTTP, HTTPS, and AJP protocols**.  
    - Allows deployment of `.war` (Web Archive) files.  
    - Lightweight and easy to configure compared to full Java EE servers.  
    - Provides a **management interface** for monitoring and configuring applications.  

- **Use Case:**  
    Tomcat is ideal for **deploying web applications written in Java**, such as REST APIs, JSP pages, or Java servlets, without needing a full Java EE application server.  

- **Typical Deployment:**  
    - Applications are packaged as `.war` files and deployed in the **`webapps/`** directory.  
    - Can run on any machine with a compatible **Java Runtime Environment (JRE)**.  
    - Configurable via **`server.xml`**, `web.xml`, and context files.  

- **Ports:**  
    - Default HTTP port: **8080** (can be changed).  
    - Default HTTPS port: **8443**.  

---

### Approach

- Connect the the ssh server:
    ```apache
    ssh banner@172.16.238.12
    ```
    pass: BigGr33n
- Install Tomcat Server:
    ```apache
    yum install tomcat -y
    ```
- Get into the Tomcat server config:
    ```apache
    vi /etc/tomcat/server.xml
    ```
    Now change the **Connector** port there from **8080** to **8084**  
    ```xml
        <Connector port="8084" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443"
               maxParameterCount="1000"
    ```
    Then save the file with `:wq`
- Now enable and start the service:
    ```apache
    systemctl enable tomcat
    systemctl start tomcat
    ```
- Verify the port os working:
    ```apache
    netstat -tulnp | grep 8084
    curl localhost:8084
    ```
- Now go to the jump host server:
    ```apache
    ssh thor@jump_host.stratos.xfusioncorp.com
    ```
    pass: mjolnir123
- Then upload the **ROOT.war** to banner:
    ```apache
    scp /tmp/ROOT.war banner@stapp03:/tmp/
    ```
- No copy thr file to the Tomcat web dir:
    ```apache
    cp /tmp/ROOT.war /var/lib/tomcat/webapps/
    ```
- Then verify again:
    ```apache
    curl localhost:8084
    ```
    Hurrah! It's working now.