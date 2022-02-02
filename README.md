# Setup SSH Tunnel (Autossh) & SSH Config

Environment:

-   Manjaro 21.2.2 (Arch Linux Base)
-   Ubuntu 20.04 (Debian Base)
-   CentOS 7 (CentOS/RHEL Base)

---

## SSH config

SSH config for Forward/Jump

```bash
nano ~/.ssh/config
```

Sample code on directory `./ssh`

---

## Install Autossh

```bash
# Arch Linux
sudo pacman -S autossh

# Ubuntu/Debian Linux
sudo apt install autossh -y

# CentOS/RHEL Linux
sudo yum makecache
sudo yum -y install autossh
```

### Create System service on User

-   Create directory systemd

    ```bash
    mkdir -p ~/.config/systemd/user
    ```

-   Create SSH tunnel service

    Sample code on directory `./tunnels`

    ```bash
    nano ~/.config/systemd/user/web.ssh.tunnel.service
    ```

    Filename: `web.ssh.tunnel.service`

    ```bash
    [Unit]
    Description=AutoSSH tunnel service to My SSH Tunnel Server on local port 2212
    After=network.target

    [Service]
    Environment="AUTOSSH_GATETIME=0"
    ExecStart=/usr/bin/autossh -M 0 -o "ServerAliveInterval 30" -o "ServerAliveCountMax 3" -N -i ~/.ssh/aws-vpc-key-pair.pem ec2-user@ec2-54-169-212-111.ap-southeast-1.compute.amazonaws.com -L 2212:ip-10-0-1-232.ap-southeast-1.compute.internal:22
    Restart=on-failure
    RestartSec=10
    TimeoutSec=10

    [Install]
    WantedBy=default.target
    ```

### Reload Systemd on User

```bash
systemctl --user daemon-reload
```

Everytime change the service script, daemon must reload

### Running SSH tunnel Service

```bash
systemctl --user enable web.ssh.tunnel
systemctl --user disable web.ssh.tunnel
systemctl --user start web.ssh.tunnel
systemctl --user stop web.ssh.tunnel
systemctl --user status web.ssh.tunnel
```

---

Do not hesitate if there are suggestions and criticisms 😃 [@asapdotid](https://github.com/asapdotid)
