How to set up a mumble server on Bluefin Linux
=================================================

Last updated: 2026-04-25

Put the server in a container. The pattern is:

1. Make a dedicated directory
2. Pull the image and run the container
3. Do some light configuration
4. Test connectivity over Tailscale

Should be up and running in 5 minutes

mkdir -p ~/services/mumble/data

podman run -d \
  --name mumble \
  --restart=unless-stopped \
  -p 64738:64738/tcp \
  -p 64738:64738/udp \
  -v ~/services/mumble/data:/data:Z \
  -e MUMBLE_CONFIG_REGISTER_NAME="Glen's Mumble Server" \
  -e MUMBLE_CONFIG_WELCOME_TEXT="Welcome to Glen's Mumble server!" \
  -e MUMBLE_CONFIG_USERS=10 \
  docker.io/mumblevoip/mumble-server:latest

Check the generated SuperUser password from the logs:
podman logs mumble | grep -i SuperUser

Connect to the server from the Mumble client:
Server: localhost
Port: 64738
Username: SuperUser
Password: [from the logs]

Ensure Tailscale is up and running:

tailscale up

Test connection:
From another machine with Tailscale running

Server: <magic Tailscale DNS of the server>
Port: 64738
Username: testuser

Creat channels as SuperUser
Lobby
Games
Study / Chill

Increase bandwidth: 
nvim ~/services/mumble/data/mumble_server_config.ini

bandwidth=96000

podman restart mumble
