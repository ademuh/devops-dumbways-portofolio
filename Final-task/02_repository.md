## Preemtpive Setup
- appserver on IDCH
- Ansible playbook scripts

## Pulling Repository
```
#cat init-repository.yaml

---
-
  become: false
  gather_facts: false
  hosts: appserver
  tasks:
    -
      name: "Initializing git config (1/2)"
      shell: "git config --global user.name <github username>"
    -
      name: "Initializing git config (2/2)"
      shell: "git config --global user.email <github email>"
    -
      become_user: ads
      git:
        dest: "{{dirb}}"
        repo: "https://github.com/ademuh/literature-backend.git"
        version: HEAD
      name: "Pulling backend repository"
    -
      become_user: ads
      git:
        dest: "{{dirf}}"
        repo: "https://github.com/ademuh/literature-frontend.git"
        version: HEAD
      name: "Pulling frontend repository"
  vars:
    -
      dirb: /home/ade/literature-be/
    -
      dirf: /home/ade/literature-fe/
   ```
   
   ansible-playbook :
   
   ![](https://github.com/ademuh/devops13-finaltask-ade/blob/main/media/1-repo1.png?raw=true)
   
   > We have created literature-fe and literature-be containing the repositories.
   
## git branch

Run :
`git branch <branch name>`
> Creates new branch

`git branch -a`
> Check available branches

 ![](https://github.com/ademuh/devops13-finaltask-ade/blob/main/media/vsc1.png?raw=true)

`git checkout <branch name>`
> Using the defined branch

 ![](https://github.com/ademuh/devops13-finaltask-ade/blob/main/media/vsc5.png?raw=true)

## git remote & git config

Run :
`git config --list` to ensure we are using the right account

 ![](https://github.com/ademuh/devops13-finaltask-ade/blob/main/media/vsc2.png?raw=true)

`git remote add <remote> <link>` to add new remote
`git remote set-url <remote> <link>` to change the url on existing remotes

![](https://github.com/ademuh/devops13-finaltask-ade/blob/main/media/vsc3.png?raw=true)
