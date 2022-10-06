# Dumbways.id DevOps 13 - Final Task

## Running Servers @ IDCloudHost
- ads-appserver
- ads-monitoring
> 2 CPU, 2 GB RAM, 20GB Storage

- ads-gateway
> 1 CPU, 1 GB RAM, 20GB Storage

## Tasks

01_user
- Create a new user under a new name `ade`
- Disabled password sign-in
- Applied to all servers

02_repository
- Using dumbwaysdev/literature
- Create 3 branches (Development, Staging & Production)
- Run the program on Production
- Connect `git remote` and `git config`

03_SSH
- Generate `ssh-keygen` on local, change alias (ade@dumbways)
- Insert all keys to all servers
- Access all servers with ssh private key

04_deployment
- Integration between front-end, back-end & database
- Running on top of Docker
- Combining them into 1 compose
- Running PostgreSQL with new database (literature)

05_webserver
- Reverse Proxy to all
- Load Balancing to front-end and back-end
- Multilevel domain wildcard using Certbot

06_CICD
- Jenkins on top of Docker
- Pipelining front-end & back-end
- Publish over SSH for front-end
- Discord webhook notification

07_monitoring
- Running prometheus & Grafana
- Specific Monitoring
- Basic auth for Prometheus
