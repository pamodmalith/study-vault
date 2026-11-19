```bash
# 1. Install Docker Engine, Compose, and Buildx
sudo pacman -Syu docker docker-compose docker-buildx

# 2. Enable and start the background daemon
sudo systemctl enable --now docker.service

# 3. Allow your user to run docker without sudo (log out and back in afterward)
sudo usermod -aG docker $USER

```

```bash
# 1. Install native Docker, Compose, Buildx, and Lazydocker from official repos
sudo pacman -Syu docker docker-compose docker-buildx lazydocker

# 2. Install rootless daemon helpers from the AUR (using yay or paru)
yay -S docker-rootless-extras

# 3. Stop and disable the system-wide root daemon if it is running
sudo systemctl disable --now docker.service docker.socket

```

