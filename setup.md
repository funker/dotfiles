# Ansible VM setup

## Proxmox Host

Clone Ubuntu Cloud Image template via web GUI

### Run as `root`

ssh into the VM from Proxmox shell

    ssh <USER>@<VM_HOSTNAME>

## VM

Add user `<USER>` to the groups `sudo`

    usermod -aG sudo <USER>

Become `<USER>`
    
    sudo su - <USER>

### Run as `<USER>`

Customize permissions to ~/.ssh

    chmod 700 ~/.ssh

Customize permissions to ~/.ssh/authorized_keys
    
    chmod 600 ~/.ssh/authorized_keys

Become root again

    exit

Edit SSH config

    nano /etc/ssh/sshd_config

Uncomment these lines and change them accordingly:

- Set `PubkeyAuthentication` to `yes` to enable login with public keys
- Set `PasswordAuthentication` to `no` to disable login with password

Restart SSH service

    systemctl restart ssh

Let all sudoers execute commands without needing a password

    sudo visudo

- Replace `%sudo   ALL=(ALL:ALL) ALL` with `%sudo   ALL=(ALL:ALL) NOPASSWD:ALL`

Save and exit with `Ctrl+X` and `y`

## Login via SSH as `<USER>`

    ssh <USER>@<IP_ADDRESS>

Install Starhip (https://starship.rs)

    curl -sS https://starship.rs/install.sh | sh

Install some tools

    apt install git stow neofetch

Remove default dotfiles

    rm -rf .bashrc .bash_aliases .hushlogin .gitconfig

Clone dotfiles repository from Github

    git clone https://github.com/funker/dotfiles.git ~/dotfiles

Stow the dotfiles

    cd ~/dotfiles
    stow .

Restart the terminal



---
---
---




# Docker host setup

## Proxmox Host

### Run as `root`

Enter the LXC container from Proxmox shell

    pct enter <ID>

## LXC container

### Run as `root`

Create user `<USER>`

    sudo adduser <USER>

Add user `<USER>` to the groups `sudo` and `docker`

    usermod -aG sudo,docker <USER>

Become `<USER>`
    
    sudo su - <USER>

### Run as `<USER>`
Create `.ssh` directory
    
    mkdir ~/.ssh

Customize the `authorized_keys` file allowing the pasted keys to access the machine without a password
    
    nano ~/.ssh/authorized_keys

Paste SSH keys from `authorized_keys` file


Save and exit with `Ctrl+X` and `y`

Customize permissions to ~/.ssh

    chmod 700 ~/.ssh

Customize permissions to ~/.ssh/authorized_keys
    
    chmod 600 ~/.ssh/authorized_keys

Become root again

    exit

Edit SSH config

    nano /etc/ssh/sshd_config

Uncomment these lines and change them accordingly:

- Set `PubkeyAuthentication` to `yes` to enable login with public keys
- Set `PasswordAuthentication` to `no` to disable login with password

Restart SSH service

    systemctl restart ssh

Let all sudoers execute commands without needing a password

    sudo visudo

- Replace `%sudo   ALL=(ALL:ALL) ALL` with `%sudo   ALL=(ALL:ALL) NOPASSWD:ALL`

Save and exit with `Ctrl+X` and `y`

---

## Login via SSH as `<USER>`

    ssh <USER>@<IP_ADDRESS>

Install Starhip (https://starship.rs)

    curl -sS https://starship.rs/install.sh | sh

Install docker

    # Add Docker's official GPG key:
    sudo apt-get update
    sudo apt-get install ca-certificates curl
    sudo install -m 0755 -d /etc/apt/keyrings
    sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
    sudo chmod a+r /etc/apt/keyrings/docker.asc

    # Add the repository to Apt sources:
    echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    sudo apt-get update

    # Install Docker
    sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

Install some tools

    apt install git stow neofetch

Create fonts directory

    mkdir -P ~/.local/share/fonts

Download and unzip Fonts

    wget -qO- https://github.com/ryanoasis/nerd-fonts/releases/download/v3.1.1/FiraCode.zip | unzip -j -d ~/.local/share/fonts - "*.ttf" - && rm -f FiraCode.zip

    wget -qO- https://github.com/ryanoasis/nerd-fonts/releases/download/v3.1.1/FiraMono.zip | unzip -j -d ~/.local/share/fonts - "*.ttf" - && rm -f FiraMono.zip

Remove default dotfiles

    rm -rf .bashrc .bash_aliases .hushlogin .gitconfig

Clone dotfiles repository from Github

    git clone https://github.com/funker/dotfiles.git ~/dotfiles

Stow the dotfiles

    cd ~/dotfiles
    stow .

Update font cache (Debian)

    fc-fache -f -v

