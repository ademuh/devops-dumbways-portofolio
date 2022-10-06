## Preemptive Setup
- 02_user has been initialized
- Visual Studio Code with VSCRemote installed (optional)

## Setting up SSH Remote

We need to create a `config` file in `.ssh/`
 
```
#cat .ssh/config

host gateway
    HostName <Gateway-Server Elastic IP>
    User username
    IdentityFile /home/username/.ssh/privatekey
    ForwardAgent yes
    AddKeysToAgent yes
    RemoteForward 52698 127.0.0.1:52698

host app
    Hostname <appserver Private IP>
    User username
    IdentityFile /home/username/.ssh/privatekey
    ForwardAgent yes
    AddKeysToAgent yes
    RemoteForward 52698 127.0.0.1:52698
    ProxyCommand ssh gateway -W %h:%p

host monitor
    Hostname <Monitoring-server Private IP>
    User username
    IdentityFile /home/username/.ssh/privatekey
    ForwardAgent yes
    AddKeysToAgent yes
    RemoteForward 52698 127.0.0.1:52698
    ProxyCommand ssh gateway -W %h:%p
```

Now, we should be able to log in into any server through `gateway` elastic IP using :
`ssh app`

