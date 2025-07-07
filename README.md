# outline-podman
Set of scripts and unit files to run Outline in podman pod instead of Docker

# Install

- Clone the repository in a local directory (as root or preferably as a local user) :
```bash
    git clone https://github.com/bmairlot/outline-podman.git && cd outline-podman    
```
- Copy the files into the correct containers/systemd directory.
  - As a root user (not recommended): 
  ```bash
    mkdir -p /etc/containers/systemd/env
    cp -r *.{container,volume,pod,network} /etc/containers/systemd  
    cp env/outline.env.example /etc/containers/systemd/env/outline.env
  NEW_PASSWORD=$(uuidgen) && sed -i "s/POSTGRES_PASSWORD=.*/POSTGRES_PASSWORD=$NEW_PASSWORD/" /etc/systemd/containers/env/outline.env
  NEW_DOMAIN="outline.mycompany.com" && sed -i "s/DOMAINS: 'outline\.[^']*' -> /DOMAINS: '$NEW_DOMAIN -> /" /etc/systemd/containers/env/outline.env
  systemctl daemon-reload
   ```
  - Or preferably as a normal user :
  ```bash
    mkdir -p ~/.config/containers/systemd
    cp -r env *.{container,volume,pod,network} ~/.config/containers/systemd
    cp env/outline.env.example ~/.config/containers/systemd/env/outline.env
  NEW_PASSWORD=$(uuidgen) && sed -i "s/POSTGRES_PASSWORD=.*/POSTGRES_PASSWORD=$NEW_PASSWORD/" ~/.config/containers/systemd/env/outline.env
  NEW_DOMAIN="outline.mycompany.com" && sed -i "s/DOMAINS: 'outline\.[^']*' -> /DOMAINS: '$NEW_DOMAIN -> /" ~/.config/containers/systemd/env/outline.env
  sudo loginctl enable-linger outline
  systemctl --user daemon-reload    
   ```
- Then start the pod
```bash
    systemc --user start outline-pod
```

# Notes

- The network definition is completely arbitrary. You may choose the subnets that you wish.
- In order to keep the user's service running after deconnexion, the command sudo loginctl enable-linger outline is required.