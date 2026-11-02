# Nvidia_on_LXC_Proxmox

Add propietary repositories non-free non-free-firmware on /etc/apt/sources.list.d/debian.sources:
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

Download you packet driver on Proxmox host
Example:
```bash
curl https://www.nvidia.com/es-es/drivers/details/281497/
 ```
```bash
chmod +x Nvidia*
 ```

Execute using ./


```bash
curl https://www.nvidia.com/es-es/drivers/details/281497/
 ```
💻:Install packets for servers(proxmox):
```bash
apt install build-essential dkms pkg-config libglvnd-dev pve-headers-$(uname -r) pkg-config libglvnd-dev
 ```
