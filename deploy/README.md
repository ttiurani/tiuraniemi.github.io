# Deployment

## Initial Setup

The following steps need to be done once when setting up the Raspberry Pi. After that, Ansible takes over.

### 1. Install Raspberry Pi OS Lite

The lite version of the OS is quite enough for hosting the site. Setup SSH access with the `pi` user with a strong password.

### 2. Reserve static internal IP for Raspberry Pi from the router

The router provides an internal IP address for the Pi, which needs to be made static so it doesn't change when to Pi boots.

### 3. Log in and find out external IP

Run:

```
ssh pi@[IP address from 2 here]
```

to access the Rasperrby pi. To get the external IP adress run:

```
curl -4 icanhazip.com
```

and log out with `exit`.

### 4. Setup Cloudflare DNS with the IP

In Cloudflare DNS settings, set A record to your domain to point to the IP address you got from 3. Set a short TTL, e.g. 1min to force refetch on dynamic IP changes.

### 5. Configure Router SSH Port Forwarding

Your router probably isn't forwarding SSH by default to to the Raspberry pi, so set up forwarding of port 22 to Raspberry Pi port 22.

### 6. Log In to Your Domain

Run:

```
ssh pi@[your domain]
```

to access the Raspberry Pi from the outside.

### 7. Setup SSH Key Access

Create new SSH keys to `~/.ssh/id_rsa_pi` with:

```
ssh-keygen
```

and then copy the file to the Raspberry Pi with:

```
ssh-copy-id -i ~/.ssh/id_rsa_pi pi@[your domain]
```

After that add to ~/.ssh/config

```
Host [your domain]
   Hostname [your domain]
   User pi
   PubkeyAuthentication yes
   Port 22
   IdentityFile ~/.ssh/id_rsa_pi
```

and you should be able to `ssh [your domain]` to log in.

## Ansible Setup

Once there is a way to access the Raspberry Pi with SSH keys from the internet, use the following playbooks to configure it.

### upgrade.yml

This playbook upgrades the debian packages on the Raspberry Pi. If kernel updates, reboot is issued.

### setup_services.yml

TODO:

https://github.com/timothymiller/cloudflare-ddns