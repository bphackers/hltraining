## Installation on a VPS

tested with Ubuntu 16.04.2 LTS
Be sure you can connect as root to your server with the password given by your VPS provider

Generate on ssh key pair called 'hltraining' in ~/.ssh on your host:

    ssh-keygen  
    ssh-copy-id -i ~/.ssh/hltraining.pub root@YOUR_SERVER

Try to connect with the key to your server from your host

    ssh -i ~/.ssh/hltraining root@YOUR_SERVER

Change the ssh port of the server in /etc/ssh/sshd_config

    Port 2222

Then restart ssh on your server

    /etc/init.d/ssh restart

Add this entry to your ~/.ssh/config file on your host

    Host hl1
        HostName YOUR_SERVER
        Port 2222
        User root
        IdentityFile ~/.ssh/hltraining

Try to connect to your server

    ssh hl1

Add the following line to /etc/hosts file on your host

    YOUR_SERVER_IP_ADDRESS     hl1

Then try to ping your server from yout host

    ping hl1

## Provisioning

From your host:

    git clone https://github.com/bphackers/hltraining.git
    cd provisioning
    ansible-playbook bootstrap.yml -i hosts -vvvv
