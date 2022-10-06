## Preemptive Setup
- 3 Servers with 6 DNS Addresses (app, api, jenkins, prometheus, exporter, grafana)
- Cloudflare API Token/ Global API Key (_not recommended_)

## Install Nginx

Ansible :
```
#cat nginx.yml

---
-
  become: true
  gather_facts: false
  hosts: gateway
  tasks:
    -
      apt:
        name: nginx
        state: latest
      name: "Installing nginx"
    -
      ansible.builtin.copy:
        dest: /etc/nginx/sites-enabled
        src: ~/ansibleapps/config/rproxy/
      name: "Sending Proxy Configuration"
    -
      name: "Restarting nginx service"
      service:
        enabled: true
        name: nginx
        state: restarted
```

There is 6 files inside the `~/ansibleapps/config/rproxy/`

- loadbal-fe.conf (Reverse Proxy Included)
- loadbal-be.conf (Reverse Proxy Included)
- grafana.conf
- prom.conf (_prometheus_)
- exp.conf (_Node Exporter_)
- jenkins.conf

loadbal-fe/loadbal-be configuration :

```
# 10.36.116.28 = Private IP appserver
# 3002-3003 Port for frontend (tunneling from 3000)

upstream front-end {
    server 10.36.116.28:3002;
    server 10.36.116.28:3003;
}
server {
    server_name <dns address>;

    location / {
             proxy_pass http://yeetus;
             proxy_set_header Host ade.studentdumbways.my.id;  # This is optional, only to define the header
      }
}
```

grafana configuration :

```
# 10.36.116.23 = Private IP monitoring
# 3001 Port for Grafana Dashboard access (tunneling from 3000)

server {
    server_name <dns address>;

        location / {
             proxy_pass http://10.36.116.23:3001;
             proxy_set_header Host $http_host; # This is important for monitoring purposes
    }
}
```

Port Configuration :
- Jenkins = 8080
- Node Exporter = 9100
- Prometheus = 9090

> If there's a conflict between the ports, we can tunnel them with docker-compose using `ports : - new port:tunnel port`

## Cloudflare DNS

Since we are using cloudflare, we can log into the account and use the domain.
In this case, we are using `studentdumbways.my.id`

![image](https://user-images.githubusercontent.com/111945026/193205512-f85b36b8-7042-4537-a003-e00990ed6cfa.png)

Then we can access the DNS on the left-hand menu.

![image](https://user-images.githubusercontent.com/111945026/193205578-697865d3-7a0f-442e-8ece-e11afc82f65d.png)

From here, we can setup the sub-domain we're using and use the Elastic IP from our gateway server.

![image](https://user-images.githubusercontent.com/111945026/193205949-8ea4fa0f-da54-4735-9345-b77a807d7582.png)

> 103.134.154.46 = Elastic IP of ads-gateway server

From here, we can create a secret API file so we can use cloudflare integrated plugins (ex: Certbot).
First we need to access our account.

![image](https://user-images.githubusercontent.com/111945026/193206376-442cc0f8-090b-492e-9555-e61c397a7241.png)

Then, we can choose API token on the left-hand menu, and then we can either use API Token (_recommended_) or Global API Key (_not recommended_).

![image](https://user-images.githubusercontent.com/111945026/193206568-dc83c83b-54fb-4315-aa4a-2b389c633404.png)

For example, we are using our API Token, then we can access our server and create a `.secret` directory and create a file with :
```
# Cloudflare API token used by Certbot
dns_cloudflare_api_token = 0123456789abcdef0123456789abcdef01234567
```

> set the file permisson to 600 or +rw-x for security purposes

## Cerbot instructions

We are required to generate a SSL Certificate so the web can be accessed through 443 (https://)

Use the wildcard instructions from Certbot :

[Certbot Instructions for Nginx and Ubuntu 20.04](https://certbot.eff.org/instructions?ws=nginx&os=ubuntufocal)

Final results :

![](https://github.com/ademuh/devops13-finaltask-ade/blob/main/media/ssl7.png?raw=true)


