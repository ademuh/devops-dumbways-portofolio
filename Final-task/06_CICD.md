## Preemtpive Setup
- Monitoring Server to run Jenkins
- Docker installed
- Reverse Proxy on Jenkins (ease of access)

## Installation
We can run either :

`docker pull jenkins/jenkins:<tag>`
`docker run jenkins/jenkins:<tag> [OPTIONS]`

Or create a compose like this :

```
version: '3.7'
services:
  jenkins:
    image: jenkins/jenkins:lts-jdk11
    privileged: true
    user: root
    ports:
      - 8080:8080
      - 50000:50000
    container_name: jenkins
    volumes:
      - /home/ade/jenkins_compose/config:/var/jenkins_home
      - /var/run/docker.sock:/var/run/docker.sock
```

Once we run `jenkins`, it is required to login into the dashboard and configure jenkins. Do not forget to aqcuire the password included inside the logs while launching jenkins

![](https://github.com/ademuh/devops13-finaltask-ade/blob/main/media/jenkins1.png?raw=true)

![](https://github.com/ademuh/devops13-finaltask-ade/blob/main/media/jenkins2.png?raw=true)


> Default login is `admin`

Since we do not require a lot of plugins, we can just choose _"Select plugins to Install"_

![](https://github.com/ademuh/devops13-finaltask-ade/blob/main/media/jenkins1-2.png?raw=true)

> SSH Agent & DiscordNotifier is installed

Once we're done picking the desired plugins, we continue the installation.

![](https://github.com/ademuh/devops13-finaltask-ade/blob/main/media/jenkins3.png?raw=true)

Once everything is done, we can start using it.
We can log-in using the URL we designated to be used (in this case, jenkins.ade.studentdumbways.my.id)

![image](https://user-images.githubusercontent.com/111945026/193210406-4e4fc8eb-6d08-414c-874b-1e47809bb806.png)

After logging in, we can start setting up the _credentials_ so we can start using jenkins.

> Use the same ssh-keygen we generated, go to `Manage Jenkins` > `Configure Credentials` > Double Click `System`
>
> `+ Add Credentials` to input our ssh-keygen, `+ Add Domain` to input the appserver IP

## Pipelining

Here, we can start create a new Pipeline task in Jenkins using a pipeline script inside the repository that we want to use.
In this case, we are using the literature-frontend repository and `Jenkinsfile` as the pipeline file

First, we create the jenkinsfile before we can start setup for pipelineing.

```
# cat Jenkinsfile

def branch = "production"
def rname = "origin"
def dir = "~/literature-fe/"
def credential = 'appserver'
def server = 'ade@103.187.146.122'
def img = 'aimingds/literature-fe'
def cont = 'literature-fe'

pipeline {
    agent any

    stages {
        stage('Repository Pull') {
            steps {
                sshagent([credential]){
                    sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
                    echo "Pulling Wayshub Backend Repository"
                    cd ${dir}
                    docker container stop ${cont}
                    docker container rm ${cont}
                    docker image rm ${img}:latest
                    git pull ${rname} ${branch}
                    exit
                    EOF"""
                }
            }
        }

        stage('Building Docker Image') {
            steps {
                sshagent([credential]){
                    sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
                    echo "Building Image"
                    cd ${dir}
                    docker build -t ${img}:${env.BUILD_ID} .
                    exit
                    EOF"""
                }
            }
        }

        stage('Image Deployment') {
            steps {
                sshagent([credential]){
                    sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
                    cd ${dir}
                    docker tag ${img}:${env.BUILD_ID} ${img}:latest
                    cd ..
                    docker-compose -f start-literature.yml up -d
                    exit
                    EOF"""
                }
            }
        }

        stage('Pushing to Docker Hub (aimingds)') {
            steps {
                sshagent([credential]){
                    sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
                    cd ${dir}
                    docker image push ${img}:latest
                    exit
                    EOF"""
                }
            }
        }
        stage('Push Notification Discord') {
           steps {
                sshagent([credential]){
                    discordSend description: "wayshub-backend:" + BUILD_ID, footer: "Ade Muhammad Safari - Dumbways.id Devops Batch 13", link: BUILD_URL, result: currentBuild.currentResult, scmWebUrl: '', title: 'Wayshub-backend', webhookURL: 'https://discord.com/api/webhooks/1024706033786028103/Bn02YoDHmtVnK2HpWxckZzCItV8LMjCcNNn5mdY-V_nCQDz-0N42R8cKevG03IurUJRH'
                }
            }
        }
    }
}
```

> In the repository pull script, you can add `git config` to insert your user.name and user.email to ensure you use your own account.

Once we created the Jenkins file, I personally use ansible-playbook to copy the files into the repository.

![jenkins7](https://user-images.githubusercontent.com/111945026/193210207-1ef8dae0-b414-4c65-a9ea-eeb463d6c9e2.png)

From here, we can start creating a new task for our pipeline, Go to the jenkins dashboard, choose `New Task` and pick the `Pipeline` option
Then we can configure the repository, credentials and the branch we are using.

![jenkins9](https://user-images.githubusercontent.com/111945026/193212142-2fbdd6e9-6ce5-4658-8f0e-e97f9b6a0aff.png)

> Turn off "Lightweight Checkout" if you want jenkins to keep building from scratch instead of using their cache

To ensure that Jenkins will run the pipeline after every push on github, we need to access our Repository settings and add the webhook.

![image](https://user-images.githubusercontent.com/111945026/193212903-21abb960-270d-4d4a-98e5-abbd36dcd5ae.png)

![image](https://user-images.githubusercontent.com/111945026/193213165-1b0a555c-0c44-4f1c-9f50-235d03aa959c.png)

From here, we should be able to trigger the build by pushing the repository.

![jenkins8](https://user-images.githubusercontent.com/111945026/193213261-e02b92f4-a343-4447-b78a-f2d860a50d44.png)

![jenkins10](https://user-images.githubusercontent.com/111945026/193213282-2f4eabf1-e597-4248-8761-e04f2f4615b3.png)

## Discordsend Notification

![image](https://user-images.githubusercontent.com/111945026/193209726-45ba2d00-1087-4159-bba4-9716f02951b7.png)

> Top : Notified that the build has been completed
>
> Bottom : Notified that the build has been triggered
