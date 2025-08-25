## <center> Day 16: Install and Configure Nginx as an LBR

```
Day by day traffic is increasing on one of the websites managed by the Nautilus production support team. Therefore, the team has observed a degradation in website performance. Following discussions about this issue, the team has decided to deploy this application on a high availability stack i.e on Nautilus infra in Stratos DC. They started the migration last month and it is almost done, as only the LBR server configuration is pending. Configure LBR server as per the information given below:



a. Install nginx on LBR (load balancer) server.

b. Configure load-balancing with the an http context making use of all App Servers. Ensure that you update only the main Nginx configuration file located at /etc/nginx/nginx.conf.

c. Make sure you do not update the apache port that is already defined in the apache configuration on all app servers, also make sure apache service is up and running on all app servers.

d. Once done, you can access the website using StaticApp button on the top bar.
```

---

### What is Load Balancing (LBR)?

Load balancing is a technique used to **distribute incoming network traffic across multiple servers**.  

- Think of it like a **traffic cop** at a busy intersection: instead of all cars going to one lane (server), the cop directs them evenly across all lanes.  
- This prevents **any single server from being overloaded** and keeps the website fast and available.


### Why is Load Balancing used?

1. **Better Performance**  
   - If one server gets too many requests, it slows down.  
   - With load balancing, requests are shared, so users experience faster responses.
2. **High Availability / Fault Tolerance**  
   - If one server goes down, the load balancer can send traffic to the remaining healthy servers.  
   - This prevents the website from going offline.
3. **Scalability**  
   - When traffic grows, you can add more servers to the pool without changing the user-facing URL.  
   - The load balancer automatically starts sending requests to the new servers.
4. **Centralized Management**  
   - You control traffic from a single point (the load balancer) instead of manually routing users.

### How it works in your setup

- You have **3 App Servers**: `stapp01`, `stapp02`, `stapp03`.  
- Nginx is your **load balancer**.  
- Users access **one URL** (StaticApp button).  
- Nginx distributes each request to one of the three servers in **round-robin fashion** (or other algorithms).  
- Each server serves the same website, so users get the content no matter which server handles the request.

### Simple Analogy

Imagine a restaurant with **3 chefs**.  

- If 100 orders come in, giving all orders to **one chef** will be slow.  
- A **head chef (load balancer)** distributes the orders evenly to all 3 chefs.  
- Everyone eats faster and no chef gets overwhelmed.

---

### Solution

- Login to LBR server:
    ```apache
    ssh loki@stlb01
    ```
    Mischi3f
- Install nginx:
    ```apache
    yum install nginx -y
    ```
- Make sure apache service is up and running on all app servers and get the ports:
    ```sh
    ssh-keygen -t rsa -b 2048
    for server in tony@stapp01 steve@stapp02 banner@stapp03;
        do ssh-copy-id $server;
    done
    for server in tony@stapp01 steve@stapp02 banner@stapp03;
        do ssh -t $server "sudo bash -c 'systemctl enable httpd; systemctl start httpd; systemctl status httpd; ss -tlnp | grep httpd'"
    done
    ```
    Ir0nM@n  
    Am3ric@  
    BigGr33n
- Edit nginx config file `/etc/nginx/nginx.conf`:
    ```apache
    cp /etc/nginx/nginx.conf /etc/nginx/nginx.conf.bak
    vi /etc/nginx/nginx.conf
    ```
    ```nginx
    http {
        upstream AppServers {
            server stapp01:3004;
            server stapp02:3004;
            server stapp03:3004;
        }

        server {
            listen 80;

            location / {
                proxy_pass http://AppServers;
                proxy_set_header Host $host;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header X-Forwarded-Proto $scheme;
            }
        }
    }
    ```
    Save and Exit with `:wq`
- Test & Start Nginx:
    ```apache
    nginx -t
    systemctl enable nginx
    systemctl start nginx
    systemctl status nginx
    ```
- Verify:
    ```apache
    curl localhost
    curl stapp01:3004
    ```
    Got same output!