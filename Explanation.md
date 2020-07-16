# Rakhmanov Ainur 4311 Practice

## Setting up server

*I'm using vds server and connect to it via ssh. So...*


### Setting up ssh

#### Client

1. Creating ssh-keys

```bash
    ssh-keygen -t rsa

    Generating public/private rsa key pair.
    Enter file in which to save the key (/home/ainur/.ssh/id_rsa):
    Created directory '/home/ainur/.ssh'.
    Enter passphrase (empty for no passphrase):
    Enter same passphrase again:
    Your identification has been saved in /home/ainur/.ssh/id_rsa
    Your public key has been saved in /home/ainur/.ssh/id_rsa.pub
    ...
```

2. Passing public key to server

```bash
    ssh-copy-id root@45.11.26.143

    /usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/ainur/.ssh/id_rsa.pub"
    The authenticity of host '45.11.26.143 (45.11.26.143)' can't be established.
    ECDSA key fingerprint is SHA256:L0jSPDlRS43Am/PfJ98PXMZPZpxMNIcTNZnY4FZeEqo.
    Are you sure you want to continue connecting (yes/no/[fingerprint])? y
    Please type 'yes', 'no' or the fingerprint: yes
    /usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
    /usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
    root@45.11.26.143's password:

    Number of key(s) added: 1

    Now try logging into the machine, with:   "ssh 'root@45.11.26.143'"
    and check to make sure that only the key(s) you wanted were added.
```

3. Logging into the server

```bash
    ssh root@45.11.26.143
    

    Welcome to Ubuntu 18.04.3 LTS (GNU/Linux 4.15.0-74-generic x86_64)

     * Documentation:  https://help.ubuntu.com
     * Management:     https://landscape.canonical.com
     * Support:        https://ubuntu.com/advantage

      System information as of Thu Jul 16 14:45:19 MSK 2020

      System load:  0.03               Processes:              94
      Usage of /:   13.4% of 19.56GB   Users logged in:        1
      Memory usage: 29%                IP address for eth0:    45.11.26.143
      Swap usage:   0%                 IP address for docker0: 172.17.0.1

    .....

    ********************************************************************************

    Welcome to RuVDS Marketplace's Docker VPS.
    To keep this VPS secure, the UFW firewall is enabled.
    All ports are BLOCKED except 22 (SSH), 2375 (Docker) and 2376 (Docker).

    .....
```

As we can see ufw is installed and enabled for us.


#### Server

4. Base setup

```bash
    adduser ainur

    Adding user `ainur' ...
    Adding new group `ainur' (1000) ...
    Adding new user `ainur' (1000) with group `ainur' ...
    Creating home directory `/home/ainur' ...
    Copying files from `/etc/skel' ...
    Enter new UNIX password:
    Retype new UNIX password:
    passwd: password updated successfully
    Changing the user information for ainur
    Enter the new value, or press ENTER for the default
        Full Name []: Ainur Rakhmanov
        Room Number []:
        Work Phone []:
        Home Phone []:
        Other []:
    
    usermod -aG sudo username
    su - ainur
```

5. Allowing http traffic

```bash
    sudo ufw allow http
```

#### Client

6. Copying docker files, configs, statics etc. Should be created before!.

```bash
    scp -r ./nginx-docker ainur@45.11.26.143:/home/ainur/docker-compose
    .....
```


> _nginx-docker_'s folder interiors could be found [here](https://github.com/ToTHXaT/Nginx-postgres-fastapi.git)



7. Copying data volume for postgresql
```bash
    scp -r /var/lib/docker/volumes/nginx-docker_postgres-data root@45.11.26.143:/var/lib/docker/volume/
    .....
```


#### Server

8. Moving into docker-compose file directory and starting all containers

```bash
    cd $HOME/docker-compose/nginx-docker/

    sudo docker-compose up
```

Docker will pull all containers and will start working.

**Server is up and running now !**

Visit it [here](http://45.11.26.143/)
