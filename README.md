# cloudflared-ssh-docker

# Setup:

## Prequisites
- Create a Cloudflare account
- Add a domain to Cloudflare

## Create a Cloudflared tunnel online
- Create a Cloudflared tunnel by going to:
    - Cloudflare -> Zero Trust -> Networks -> Tunnels -> Create Tunnel -> Select Cloudflared
    - Name your tunnel
   - Copy either of the installation instructions. Save the long token at the end to use in the later steps.
   - Configure:
        ```
        subdomain: anything you want
        domain: one of your domains
        Type: SSH
        URL: localhost:<your port for ssh (default is 22)
        ```
   - Save the tunnel

## Set Up the Cloudflared tunnel locally
 - On the machine you want to access, ensure that docker is install (see [docker installation instructions](https://docs.docker.com/engine/install/)
- Pull down this github repo: `git@github.com:JohnYangSam/cloudflared-ssh-docker.git`
 - Create a .env file with:
   `CLOUDFLARED_TUNNEL_TOKEN=<tunnel token from your cloudlfared dashboard>`
 - `docker compose up -d` to run the tunnel in detached mode

## To SSH into the tunnel
- On your local machine, install cloudlfared (see [Cloudflare instructions](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/downloads/))
- On your local machine, set up `~/.ssh/config`:
    ```
    # examplehostname
    Host examplehostname
    Hostname <subdomain.maindomain.tld from your cloudflared setup>
    IdentityFile <your identity file if you are using one>
    ProxyCommand cloudflared access ssh --hostname %h
    User <your user>
    Port <your port>
    ```

- SSH with: `ssh examplehostname`
