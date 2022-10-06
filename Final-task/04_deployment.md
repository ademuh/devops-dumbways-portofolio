## Preemptive Setup
- Appserver with Public IP
- Integration with Private IP
- npm build on frontend (required for production)

## Docker Installation
```
#cat docker.yml

---
-
  become: true
  gather_facts: false
  hosts: appserver, monitor
  tasks:
    -
      apt:
        update_cache: true
      name: "Updating Server"
    -
      apt:
        name:
          - ca-certificates
          - curl
          - gnupg
          - lsb-release
      name: "Docker Dependencies"
    -
      apt_key:
        url: "https://download.docker.com/linux/ubuntu/gpg"
      name: "Docker GPG Key"
    -
      apt_repository:
        repo: "deb https://download.docker.com/linux/ubuntu focal stable"
      name: "Docker Repository"
    -
      apt:
        name:
          - docker-ce
          - docker-ce-cli
          - containerd.io
          - docker-compose-plugin
      name: "Docker Engine"
    -
      apt:
        name: python3-pip
        state: latest
        update_cache: true
      become: true
      name: "Python Dependencies"
    -
      name: "Docker SDK for Python (1/2)"
      pip:
        name: docker
    -
      name: "Docker SDK for Python (2/2)"
      shell: "sudo pip install docker-compose"
    -
      name: "Docker Group Add [ade]"
      shell: "sudo usermod -aG docker {{user}}"
  vars:
    user: ade
```

ansible-playbook :

![](https://github.com/ademuh/devops13-finaltask-ade/blob/main/media/docker1.png?raw=true)

## Dockerfile 

Front-End
```
FROM node:16.17.1-alpine
WORKDIR /app
COPY /build .
RUN npm install -g serve
EXPOSE 3000
CMD ["serve","i","."]
```

Back-End
```
FROM node:16.17.1-alpine

ENV NODE_ENV production
ENV DATABASE_URL postgresql://ade:katasandi@ade.studentdumbways.my.id:5432/literature

WORKDIR /app
COPY . .

RUN npm install
RUN npm install sequelize-cli -g
RUN npx sequelize db:migrate --env production
EXPOSE 5000
CMD ["node","server.js"]
```

Building image (aimingds/literature-fe) :
`docker build -t <username>/<image>`

![](https://github.com/ademuh/devops13-finaltask-ade/blob/main/media/docker2.png?raw=true)

![](https://github.com/ademuh/devops13-finaltask-ade/blob/main/media/docker3.png?raw=true)

Pushing image to Docker Hub :
`docker push <image>`

![](https://github.com/ademuh/devops13-finaltask-ade/blob/main/media/dokcer5.png?raw=true)

## Docker Compose

```version: '3.7'
services:
 backend:
   depends_on:
     - database
   container_name: literature-be
   image: aimingds/literature-be
   stdin_open: true
   ports:
     - 5002:5000
     - 5003:5000

 database:
   container_name: database
   image: postgres
   ports:
     - 5432:5432
   volumes:
     - ~/literature-db/postgresql:/var/lib/postgresql/data
   environment:
     - POSTGRES_USER=ade
     - POSTGRES_PASSWORD=katasandi
     - POSTGRES_DB=literature

 frontend:
   container_name: literature-fe
   image: aimingds/literature-fe
   stdin_open: true
   ports:
     - 3002:3000
     - 3003:3000
```
> Running 3002-3003 & 5002-5003 for Load Balancing in Local
> 
> **DATABASE** has to be running to build the Back-End image
> 
> Since we already defined `DATABASE_URL` (ENV DATABASE_URL postgresql://ade:katasandi@ade.studentdumbways.my.id:5432/literature) it's not required to put it on the compose

ansible-playbook :

![](https://github.com/ademuh/devops13-finaltask-ade/blob/main/media/docker5.png?raw=true)

![](https://github.com/ademuh/devops13-finaltask-ade/blob/main/media/docker6.png?raw=true)
