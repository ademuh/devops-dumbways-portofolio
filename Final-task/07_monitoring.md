## Preemtpive Setup
- Monitoring Server
- All needed ports allowed through ufw
- Reverse Proxied all sites

## Installing Node Exporter

In this case, we're only going to monitor both appserver and monitoring.
Before we can run prometheus and grafana, we are required to install node exporter to all the available servers that we wanted to monitor.

```
#cat start-exporter.yml

---
-
  hosts: appserver,monitor
  name: "Installing Node Exporter"
  tasks:
    -
      file:
        path: ~/monitoring/
        state: directory
    -
      copy:
        dest: "{{remote}}/node-exporter.yml"
        src: "{{locale}}/config/node-exporter.yml"
      name: "Sending compse.yml"
    -
      docker_compose:
        files:
          - node-exporter.yml
        project_name: Monitoring
        project_src: "{{remote}}"
      name: "Run Docker Compose"
  vars:
    -
      locale: /home/ubuntu/ansibleapps
    -
      remote: /home/ade/monitoring
 ```

docker-compose for exporter :
```
#cat node-exporter.yml

version: '3.7'
services:
  nodexp:
    command:
      - "--web.listen-address=:9100"
      - "--path.procfs=/host/proc"
      - "--path.sysfs=/host/sys"
      - "--path.rootfs=/rootfs"
      - "--collector.filesystem.ignored-mount-points='^/(sys|proc|dev|host|etc|rootfs/var/lib/docker/containers|rootfs/var/lib/docker/overlay2|rootfs/run/docker/netns|rootfs/var/lib/docker/aufs)($$|/)'"
    expose:
      - "9100"
    image: prom/node-exporter
    ports:
      - "9100:9100"
    volumes:
      - "/proc:/host/proc:ro"
      - "/sys:/host/sys:ro"
      - "/:/rootfs:ro"
```

ansible-playbook :

![](https://github.com/ademuh/devops13-finaltask-ade/blob/main/media/exp2.png?raw=true)

Chceking for prom/exporter on containers :

![](https://github.com/ademuh/devops13-finaltask-ade/blob/main/media/exp1.png?raw=true)

## Prometheus and Grafana
```
#cat start-monitoring

---
-
  hosts: monitor
  name: "Installing Server Monitoring"
  tasks:
    -
      copy:
        dest: "{{remote}}/prometheus.yml"
        src: "{{locale}}/config/prometheus.yml"
      name: "Sending Prometheus configuration (1/2)"
    -
      copy:
        dest: "{{remote}}/web.yml"
        src: "{{locale}}/config/web.yml"
      name: "Sending Prometheus configuration (2/2)"
    -
      copy:
        dest: "{{remote}}/compose-monitoring.yml"
        src: "{{locale}}/config/compose-monitoring.yml"
      name: "Send compose.yml to server"
    -
      community.docker.docker_compose:
        files:
          - compose-monitoring.yml
        project_name: Monitoring
        project_src: "{{remote}}"
      name: "Run Docker Compose"
  vars:
    -
      locale: /home/ubuntu/ansibleapps
    -
      remote: /home/ade/monitoring
```

docker-compose :
```
#cat config/compose-monitoring.yml

---
volumes:
  prometheus-data: {}
  grafana-data: {}
services:
  grafana:
    container_name: grafana
    expose:
      - "3001"
    image: grafana/grafana
    ports:
      - "3001:3000"
    stdin_open: true

  prometheus:
    container_name: prometheus
    expose:
      - "9090"
    image: "prom/prometheus:latest"
    ports:
      - "9090:9090"
    stdin_open: true
    volumes:
      - "/home/ade/monitoring/web.yml:/etc/prometheus/web.yml"
      - "/home/ade/monitoring/prometheus.yml:/etc/prometheus/prometheus.yml"
      - "prometheus-data:/prometheus"
    command:
      - '--web.config.file=/etc/prometheus/web.yml'
      - '--config.file=/etc/prometheus/prometheus.yml'

version: "3.7"
```

