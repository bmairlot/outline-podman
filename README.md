# outline-podman
Set of scripts and unit files to run Outline in podman pod instead of Docker

# Install

- Clone the repository in a local directory (as root or preferably as a local user) :
```bash
    git clone https://github.com/bmairlot/outline-podman.git
    cd outline-podman
```
- Copy the files into the correct containers/systemd directory.
  - As a root user (not recommended): 
  ```bash
    mkdir -p /etc/containers/systemd/env
    cp -r *.{container,volume,pod,network} /etc/containers/systemd  
    cp env/outline.env.example /etc/containers/systemd/env
    systemctl daemon-reload
   ```
  - As a normal user :
  ```bash
    mkdir -p ~/.config/containers/systemd
    cp -r env *.{container,volume,pod,network} ~/.config/containers/systemd
    cp env/outline.env.example ~/.config/containers/systemd/env
    systemctl --user daemon-reload
   ```
- Change the password of the database in env/outline.env (POSTGRES_PASSWORD)
```bash
NEW_PASSWORD=$(uuidgen) && sed -i "s/POSTGRES_PASSWORD=.*/POSTGRES_PASSWORD=$NEW_PASSWORD/" env/outline.env
```
- Change the domain to host to your domain in env/outline.en (DOMAINS)
```bash
 NEW_DOMAIN="outline.mycompany.com" && sed -i "s/DOMAINS: 'outline\.[^']*' -> /DOMAINS: '$NEW_DOMAIN -> /" env/outline.env
```


# Notes

The network definition is completely arbitrary. You may choose the subnets that you wish.