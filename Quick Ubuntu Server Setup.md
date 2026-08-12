# Quick Ubuntu Server Setup

A repeatable baseline for provisioning a new Ubuntu server.

This guide establishes a clean, secure, and comfortable starting environment before installing any project-specific software.

Tested with:

- Ubuntu 24.04 LTS
- DigitalOcean Droplet
- SSH key authentication
- Non-root administrative user
- UFW
- Neovim
- tmux
- Git
- ripgrep
- fd
- Swap

Once this guide is complete, the server is ready for project-specific provisioning such as Nuxt, Laravel, databases, Caddy, workers, and application services.

---

# 1. Create the Server

For a small application or marketing site, a basic DigitalOcean Droplet is generally sufficient.

Example configuration:

```text
Distribution: Ubuntu 24.04 LTS
Plan:         Basic / Shared CPU
CPU:          1 vCPU
Memory:       1 GB
Disk:         ~25 GB+
Authentication: SSH key
```

Choose resources based on the actual application.

## Add Your SSH Key During Creation

Prefer adding the workstation's existing SSH public key when creating the server.

The default Ed25519 public key is usually:

```text
~/.ssh/id_ed25519.pub
```

Display it with:

```bash
cat ~/.ssh/id_ed25519.pub
```

The `.pub` file is safe to provide to servers.

**Never share the private key:**

```text
~/.ssh/id_ed25519
```

---

# 2. Initial SSH Connection

Connect as root using the server's IP address or hostname:

```bash
ssh root@example.com
```

On the first connection, SSH may display:

```text
The authenticity of host 'example.com' can't be established.
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

Verify the fingerprint when appropriate and enter:

```text
yes
```

The server will be added to:

```text
~/.ssh/known_hosts
```

---

# 3. Update the System

Immediately update a new server:

```bash
apt update && apt upgrade -y
```

## SSH Configuration Upgrade Prompt

On DigitalOcean Ubuntu images, an upgrade may report that:

```text
/etc/ssh/sshd_config
```

has been locally modified and that a newer package version is available.

Unless there is a specific reason to replace it, choose:

```text
keep the local version currently installed
```

DigitalOcean may have intentionally modified the SSH configuration supplied with the image.

Replacing a working SSH configuration during a remote upgrade can also create unnecessary risk.

---

# 4. Reboot

After the initial upgrade completes:

```bash
reboot
```

The SSH connection will close.

Wait for the machine to restart and reconnect:

```bash
ssh root@example.com
```

Confirm the machine is running normally:

```bash
uname -r
uptime
```

---

# 5. Create a Non-Root Administrative User

Do not use `root` as the normal administrative account.

Create the user:

```bash
adduser electronbabies
```

Follow the password prompt.

The additional user information fields can be left blank if desired.

Add the user to the `sudo` group:

```bash
usermod -aG sudo electronbabies
```

Verify:

```bash
id electronbabies
```

The output should include:

```text
sudo
```

---

# 6. Configure SSH for the New User

If the root account already contains the correct workstation public key, copy its `authorized_keys` file.

As root:

```bash
mkdir -p /home/electronbabies/.ssh

cp /root/.ssh/authorized_keys \
    /home/electronbabies/.ssh/authorized_keys
```

Set ownership:

```bash
chown -R electronbabies:electronbabies \
    /home/electronbabies/.ssh
```

Set secure permissions:

```bash
chmod 700 /home/electronbabies/.ssh
chmod 600 /home/electronbabies/.ssh/authorized_keys
```

## Verify Before Closing the Root Session

**Keep the existing root SSH connection open.**

Open another local terminal and connect as the new user:

```bash
ssh electronbabies@example.com
```

Verify the user:

```bash
whoami
```

Expected:

```text
electronbabies
```

Verify sudo:

```bash
sudo whoami
```

Expected:

```text
root
```

Only after both work should the original root connection be considered unnecessary.

---

# 7. Install Baseline CLI Tools

Install the common tools used when administering and developing on servers:

```bash
sudo apt install -y \
    git \
    curl \
    wget \
    unzip \
    tmux \
    htop \
    ripgrep \
    fd-find
```

These provide:

| Package | Purpose |
|---|---|
| `git` | Source control |
| `curl` | HTTP requests/downloads |
| `wget` | File downloads |
| `unzip` | Archive extraction |
| `tmux` | Persistent terminal sessions |
| `htop` | Process/resource monitoring |
| `ripgrep` | Fast recursive text search (`rg`) |
| `fd-find` | Fast filesystem search |

Verify important tools:

```bash
git --version
tmux -V
rg --version
fdfind --version
```

---

# 8. Install Neovim

## Do Not Use Ubuntu's Neovim Package

Ubuntu 24.04 currently provides an old Neovim release through:

```bash
sudo apt install neovim
```

During the initial setup documented here, this installed:

```text
NVIM v0.9.5
```

Modern Neovim configurations and plugins may require substantially newer versions.

If the Ubuntu package was installed, remove it:

```bash
sudo apt remove -y neovim
sudo apt autoremove -y
```

## Install Current Stable Neovim

Download the official Linux x86-64 release:

```bash
cd /tmp

curl -LO \
    https://github.com/neovim/neovim/releases/latest/download/nvim-linux-x86_64.tar.gz
```

Remove any previous installation at the target location:

```bash
sudo rm -rf /opt/nvim-linux-x86_64
```

Extract Neovim under `/opt`:

```bash
sudo tar -C /opt -xzf nvim-linux-x86_64.tar.gz
```

Expose the executable system-wide:

```bash
sudo ln -sf \
    /opt/nvim-linux-x86_64/bin/nvim \
    /usr/local/bin/nvim
```

Verify:

```bash
which nvim
nvim --version | head -n 3
```

Example tested result:

```text
/usr/local/bin/nvim

NVIM v0.12.4
Build type: Release
LuaJIT 2.1.1774638290
```

---

# 9. Make `vim` Open Neovim

Rather than maintaining a shell-specific alias, expose Neovim as `vim` system-wide:

```bash
sudo ln -sf \
    /usr/local/bin/nvim \
    /usr/local/bin/vim
```

Verify:

```bash
which vim
vim --version | head -n 1
```

Expected:

```text
/usr/local/bin/vim
NVIM v0.12.4
```

Both commands can now be used:

```bash
nvim file.txt
```

or:

```bash
vim file.txt
```

---

# 10. Normalize the `fd` Command

Ubuntu/Debian installs the `fd` utility through the package:

```text
fd-find
```

but names the executable:

```text
fdfind
```

Many development tools expect the conventional command:

```text
fd
```

Create a system-wide compatibility symlink:

```bash
sudo ln -sf "$(which fdfind)" /usr/local/bin/fd
```

Verify:

```bash
which fd
fd --version
```

Example:

```text
/usr/local/bin/fd
fdfind 9.0.0
```

The version output may still identify itself as `fdfind`. This is normal.

---

# 11. Configure UFW

Check the initial firewall state:

```bash
sudo ufw status verbose
```

A fresh installation may report:

```text
Status: inactive
```

## Allow SSH First

**Never enable a remote server's firewall before ensuring SSH is allowed.**

Add the OpenSSH rule:

```bash
sudo ufw allow OpenSSH
```

Check it:

```bash
sudo ufw status
```

UFW may still report:

```text
Status: inactive
```

This is expected. The rule has been created, but the firewall has not yet been enabled.

## Enable UFW

```bash
sudo ufw enable
```

UFW will warn:

```text
Command may disrupt existing ssh connections. Proceed with operation (y|n)?
```

Enter:

```text
y
```

Verify:

```bash
sudo ufw status verbose
```

A baseline server should show SSH allowed and incoming connections denied by default.

Example:

```text
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), disabled (routed)

To                         Action      From
--                         ------      ----
22/tcp (OpenSSH)           ALLOW IN    Anywhere
22/tcp (OpenSSH (v6))      ALLOW IN    Anywhere (v6)
```

## Verify SSH Again

**Keep the existing SSH connection open.**

From another terminal:

```bash
ssh electronbabies@example.com
```

Confirm the new connection succeeds before closing the original session.

This provides a recovery path if the firewall configuration is incorrect.

---

# 12. Add Swap

Small cloud servers may ship without swap.

Check:

```bash
swapon --show
free -h
```

For example, a 1 GB DigitalOcean Droplet initially reported:

```text
Mem:   ~961 MiB
Swap:       0 B
```

A 2 GB swap file provides useful protection against temporary memory spikes from tasks such as application builds.

## Create a 2 GB Swap File

```bash
sudo fallocate -l 2G /swapfile
```

Restrict permissions:

```bash
sudo chmod 600 /swapfile
```

Initialize it:

```bash
sudo mkswap /swapfile
```

Enable it:

```bash
sudo swapon /swapfile
```

## Verify Before Making It Permanent

```bash
swapon --show
free -h
```

Example:

```text
NAME      TYPE SIZE USED PRIO
/swapfile file   2G   0B   -2
```

and:

```text
Swap: 2.0Gi
```

This follows the preferred pattern:

```text
Create → Test → Persist
```

## Persist Swap Across Reboots

Once the swap file has been verified:

```bash
echo '/swapfile none swap sw 0 0' \
    | sudo tee -a /etc/fstab
```

Verify:

```bash
tail -n 5 /etc/fstab
```

The following line should exist:

```text
/swapfile none swap sw 0 0
```

---

# 13. Opening Web Ports

This step is **not necessary for a generic SSH-only server**.

If the machine will host HTTP/HTTPS traffic, allow ports 80 and 443:

```bash
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

Verify:

```bash
sudo ufw status
```

Example web-server configuration:

```text
OpenSSH                    ALLOW       Anywhere
80/tcp                     ALLOW       Anywhere
443/tcp                    ALLOW       Anywhere

OpenSSH (v6)               ALLOW       Anywhere (v6)
80/tcp (v6)                ALLOW       Anywhere (v6)
443/tcp (v6)               ALLOW       Anywhere (v6)
```

Do not open ports simply because they might eventually be useful.

Only expose services the server actually needs.

---

# 14. Baseline Verification

At this point, verify the important pieces.

## Identity

```bash
whoami
sudo whoami
```

Expected:

```text
electronbabies
root
```

## Operating System

```bash
cat /etc/os-release
```

## Resources

```bash
free -h
df -h /
```

## Swap

```bash
swapon --show
```

## Firewall

```bash
sudo ufw status verbose
```

## Development / Administration Tools

```bash
git --version
nvim --version | head -n 1
vim --version | head -n 1
tmux -V
rg --version | head -n 1
fd --version
```

---

# 15. Generic Server Bootstrap Complete

At this point the generic server baseline is complete.

The machine has:

```text
Ubuntu 24.04 LTS
│
├── Current system packages
├── Non-root sudo administrator
├── SSH key authentication
├── UFW firewall
├── Swap
│
├── Git
├── curl
├── wget
├── unzip
├── htop
├── tmux
├── ripgrep
├── fd
│
└── Neovim
    ├── current stable release
    └── vim → nvim
```

**Stop here before adding application-specific infrastructure.**

The next documentation should describe the application's actual stack rather than bloating the generic server bootstrap.

Examples:

```text
Nuxt Production Server Setup
├── Caddy
├── Node.js
├── pnpm
├── Application directory
├── systemd service
└── Deployment

Laravel Production Server Setup
├── Caddy
├── PHP
├── Composer
├── Database
├── Queue workers
├── Scheduler
└── Deployment
```

---

# Appendix: Nuxt Server Components Tested After Bootstrap

These steps are **not part of the generic Ubuntu baseline**, but were performed immediately afterward while provisioning the SugaCoded Nuxt server.

They are recorded here until the dedicated Nuxt production documentation is complete.

## Caddy

Caddy was installed from its official Debian/Ubuntu repository.

Prerequisites:

```bash
sudo apt install -y \
    debian-keyring \
    debian-archive-keyring \
    apt-transport-https \
    curl
```

Add the signing key:

```bash
curl -1sLf \
    'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' \
    | sudo gpg --dearmor \
    -o /usr/share/keyrings/caddy-stable-archive-keyring.gpg
```

Add the repository:

```bash
curl -1sLf \
    'https://dl.cloudsmith.io/public/caddy/stable/debian.deb.txt' \
    | sudo tee /etc/apt/sources.list.d/caddy-stable.list
```

Set readable permissions:

```bash
sudo chmod o+r \
    /usr/share/keyrings/caddy-stable-archive-keyring.gpg

sudo chmod o+r \
    /etc/apt/sources.list.d/caddy-stable.list
```

Install:

```bash
sudo apt update
sudo apt install -y caddy
```

Verify:

```bash
caddy version
systemctl status caddy --no-pager
```

Tested version:

```text
Caddy v2.11.4
```

The service should report:

```text
Active: active (running)
```

---

## Node.js 22

Node.js 22 was installed through NodeSource:

```bash
curl -fsSL https://deb.nodesource.com/setup_22.x \
    | sudo -E bash -

sudo apt install -y nodejs
```

Verify:

```bash
node --version
npm --version
```

Tested versions:

```text
Node: v22.23.2
npm:  10.9.8
```

---

## pnpm

Enable Corepack:

```bash
sudo corepack enable
```

Activate the current pnpm release:

```bash
corepack prepare pnpm@latest --activate
```

Verify:

```bash
pnpm --version
which pnpm
```

Tested result:

```text
pnpm 11.21.0
/usr/bin/pnpm
```

These application-stack instructions should eventually move into a dedicated **Nuxt Production Server Setup** document once the complete deployment procedure has been established and tested.