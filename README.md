# Nvidia_on_LXC_Proxmox



Add Debian13 propietary repositories non-free non-free-firmware on /etc/apt/sources.list.d/debian.sources:
```bash
nano /etc/apt/sources.list.d/debian.sources 
```
```conf
Types: deb
URIs: http://deb.debian.org/debian/
Suites: trixie trixie-updates
Components: main contrib non-free non-free-firmware
Signed-By: /usr/share/keyrings/debian-archive-keyring.gpg

Types: deb
URIs: http://security.debian.org/debian-security/
Suites: trixie-security
Components: main contrib non-free non-free-firmware
Signed-By: /usr/share/keyrings/debian-archive-keyring.gpg
 ```
☑️ Add no subscriptions porxmox reositorories

☑️ Refresh and update proxmox

💻:Install packets for servers(proxmox):
Necesary to build and compiling driver
```bash
apt install build-essential dkms pkg-config libglvnd-dev pve-headers-$(uname -r) pkg-config libglvnd-dev
 ```

Download your nvidia driver on Proxmox host
Example:
```bash
curl https://www.nvidia.com/es-es/drivers/details/281497/
 ```
```bash
chmod +x Nvidia*
 ```

Execute using ./ and select choices in the tutorial

```bash
reboot
 ```
