## Preemptive Setup
- multipass on local
- Ansible is installed
- inventory.ini
> appserver, gateway & monitoring Public IP
> using ade, no password (except for `sudo`)
- inventory-host
> appserver, gateway & monitoring Public IP
> using ads (initial user)

## User Creation [ade]

```
#cat user-ade-add.yml


---
-
  become: true
  gather_facts: false
  hosts: all
  tasks:
    -
      name: "Creating User [ade]"
      user:
        append: true
        createhome: true
        groups: "sudo,admin"
        name: "{{newuser}}"
        password: "{{passwot}}"
        shell: /bin/bash
        state: present
        system: true
    -
      ansible.posix.authorized_key:
        key: "{{ lookup('file', '{{pubkey}}') }}"
        user: "{{newuser}}"
      name: "Adding Public Key to Authorized Keys"
    -
      copy:
        dest: "{{remot}}.ssh"
        src: "{{local}}/.secret/kunci-ads"
      name: "Sending Private Key to Servers"
    -
      lineinfile:
        line: "PasswordAuthentication no"
        path: /etc/ssh/sshd_config
        regexp: "PasswordAuthentication yes"
      name: "Disabling Password Authentication"
    -
      lineinfile:
        line: "PubkeyAuthentication yes"
        path: /etc/ssh/sshd_config
        regexp: "#PubkeyAuthentication yes"
      name: "Enabling Public Key Authentication"
    -
      name: "Reloading SSHD"
      systemd:
        name: sshd
        state: reloaded
  vars:
    newuser: ade
    passwot: $6$TaSzosd339yI$Hh/y4YtxryoOnrESQZ2U61ebCSh/DvCYGatsmZ/TnZlC4RJdkLbGturqL0GTzu1wBBBk5VAuU.ymEuByDbJoa1
    pubkey: /home/ubuntu/ansibleapps/.secret/kunci-ads.pub
    local: /home/ubuntu/ansibleapps/
    remot: /home/ade/
-
  become: true
  gather_facts: false
  hosts: appserver
  tasks:
    -
      copy:
        dest: "/home/ade/.ssh/id_rsa"
        src: "/home/ubuntu/ansible/config/.secret/kunci-ads"
      name: "Applying private key [git SSH]"
```

To run, use `ansible-playbook -i inventory-host user-ade-add.yml`

![](https://github.com/ademuh/devops13-finaltask-ade/blob/main/media/2-user1.png?raw=true)

After Setup w/ Github connections :

![](https://github.com/ademuh/devops13-finaltask-ade/blob/main/media/2-user2.png?raw=true)

