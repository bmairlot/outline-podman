# outline-podman
Set of scripts and unit files to run Outline in podman pod instead of Docker

# Install

Clone the repository in a local directory (as root or preferably as a local user) then :
- Change the password of the database in env/outline.env (POSTGRES_PASSWORD)
- Change the domain to host to your domain in env/outline.en (DOMAINS)

# Notes

The network definition is completely arbitrary. You may choose the subnets that you wish.