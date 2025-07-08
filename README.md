# outline-podman
Set of scripts and unit files to run Outline in podman pod instead of Docker

# Install

- Clone the repository in a local directory (as root or preferably as a local user) :
```bash
    git clone https://github.com/bmairlot/outline-podman.git && cd outline-podman    
```
- Copy the files into the correct containers/systemd directory as a normal user :
```bash
mkdir -p ~/.config/containers/systemd
cp -r env *.{container,volume,pod,network} ~/.config/containers/systemd
cp env/.env.sample ~/.config/containers/systemd/env/.env
cp env/postgresql.env.sample ~/.config/containers/systemd/env/postgresql.env
 ```
- (Optional, but recommended) Then change the default password to something randomized 
 ```bash
NEW_PASSWORD=$(uuidgen) && sed -i "s/POSTGRES_PASSWORD=.*/POSTGRES_PASSWORD=$NEW_PASSWORD/" ~/.config/containers/systemd/env/postgresql.env && sed -i "/DATABASE_URL/s/user:pass/user:$NEW_PASSWORD/" ~/.config/containers/systemd/env/.env
sed -i "/POSTGRES_USER=/s/=user/=outline/" ~/.config/containers/systemd/env/postgresql.env && sed -i "/DATABASE_URL/s/user:/outline:/" ~/.config/containers/systemd/env/.env
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