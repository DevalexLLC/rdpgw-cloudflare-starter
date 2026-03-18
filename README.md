# rdpgw-cloudflare-starter

Making it easier to get started with [rdpgw](https://github.com/bolkedebruin/rdpgw).

## Prerequesites

- A domain hosted on Cloudflare
- A computer or VM with Docker and Docker Compose installed
    - Must be on a separate computer (not the Windows computer you are accessing remotely)
    - We will refer to this computer your Gateway

## Installation and Setup

### Clone Repository

1. Clone this repository on your Gateway

### Cloudflare Tunnel

1. Log in to [Cloudflare One](https://one.dash.cloudflare.com/), and select 'Networks' > 'Connectors'
2. Create a tunnel
3. Choose 'Cloudflared'
4. Name your tunnel
5. Follow the instructions to install and run the connector on your Gateway
6. Pick your Hostname e.g., `gateway.example.com`; this will be your `SERVER_HOSTNAME`
7. Use `https://localhost:443` for the Service and click 'Complete Setup'

> [!TIP]
> You can see your tunnels as DNS records in your regular Cloudflare Dashboard.

### Cloudflare Origin Certs

1. Go to your [Cloudflare Dashboard](https://dash.cloudflare.com), and select your domain
2. Go to 'SSL/TLS' (use the search bar for SSL if you need to), then click 'Origin Server' > 'Create Certificate'
3. Accept the defaults and click 'Create'
4. Copy both the Certificate as `cloudflare.pem` and the Key as `cloudflare.key`; place them in the [certs](/certs/) folder

### Environment Variables

1. Copy [.env.example](/.env.example) to `.env`
2. Set your temporary admin credentials for Keycloak
3. Set `SERVER_HOSTNAME` to the hostname from [Cloudflare Tunnel step #6](#cloudflare-tunnel)
4. Set `RDP_HOSTNAME` to the hostname of your Windows computer

### Compose Up

1. You should have the following tasks complete:
    - [x] A working cloudflare tunnel
    - [x] Cloudflare cert and key saved to the [certs](/certs/) folder
    - [x] Your own `.env` file configured
2. Once those are all done, run `docker compose up -d` to start the Gateway

### Add Keycloak User

1. Go to `https://SERVER_HOSTNAME/auth/admin/master/console` and log in with your Keycloak bootstrap credentials
2. Click 'Manage realms'
3. Click 'rdpgw'
4. Click 'Users'
5. Click 'Create new user' to add your first user
6. Fill in your username (for convenience, use your Windows username; you get get it with `$env:USERNAME` in PowerShell) and click 'Create'
7. Click on your user
8. Click 'Credentials'
9. Click 'Set password'
10. Enter your password and confirm it (uncheck 'Temporary', or else you will be forced to change your password when you log in), then click 'Save'

### Download Your RDP File

Now that your user is created, you can navigate to `https://SERVER_HOSTNAME` and log in with your new user. The RDP file will automatically download once you are authenticated.

## Customizing your RDP session

The default RDP template has been customized for speed and simplicity. You can edit the options in [default.rdp](/rdpgw/default.rdp) to your liking. A good reference for the options is available at https://www.donkz.nl/overview-rdp-file-settings/. Be sure to rebuild the `rdpgw` container after you make any changes.
