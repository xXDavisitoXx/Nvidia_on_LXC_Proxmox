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
☑️ Add no subscriptions porxmox repositorories

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

Execute using ./ and select choices in the tutorial (autorize the deactivate noveau)
```bash
reboot
 ```

Test nvidia drivers:
```bash
nvidia-smi && lsmod | grep nvidia && dkms status
 ```
```conf
+-----------------------------------------------------------------------------------------+
| NVIDIA-SMI 580.196.07             Driver Version: 580.196.07     CUDA Version: 13.0     |
+-----------------------------------------+------------------------+----------------------+
| GPU  Name                 Persistence-M | Bus-Id          Disp.A | Volatile Uncorr. ECC |
| Fan  Temp   Perf          Pwr:Usage/Cap |           Memory-Usage | GPU-Util  Compute M. |
|                                         |                        |               MIG M. |
|=========================================+========================+======================|
|   0  NVIDIA GeForce GTX 970         Off |   00000000:85:00.0 Off |                  N/A |
| 71%   36C    P0             42W /  250W |       0MiB /   4096MiB |      0%      Default |
|                                         |                        |                  N/A |
+-----------------------------------------+------------------------+----------------------+

+-----------------------------------------------------------------------------------------+
| Processes:                                                                              |
|  GPU   GI   CI              PID   Type   Process name                        GPU Memory |
|        ID   ID                                                               Usage      |
|=========================================================================================|
|  No running processes found                                                             |
+-----------------------------------------------------------------------------------------+
nvidia_uvm           1986560  0
nvidia_drm            131072  0
nvidia_modeset       1687552  1 nvidia_drm
nvidia              105353216  3 nvidia_uvm,nvidia_drm,nvidia_modeset
drm_ttm_helper         20480  1 nvidia_drm
video                  77824  1 nvidia_modeset
nvidia/580.196.07, 7.0.14-26-pve, x86_64: installed

 ```
LXC: 

create your LXC first
```bash
apt update
apt dist-upgrade
apt install lshw
reboot
```
and shutdown the machine

Edit your LXC
If you your lxc have ID 130 you edit:
```bash
nano /etc/pve/lxc/130.conf
```
And change for your routes
```conf
lxc.mount.entry: /usr/bin/nvidia-smi usr/bin/nvidia-smi none bind,optional,create=file
lxc.mount.entry: /usr/lib/x86_64-linux-gnu/libnvidia-ml.so.580.196.07 usr/lib/x86_64-linux-gnu/libnvidia-ml.so.580.196.07 none bind,optional,create=file
lxc.mount.entry: /usr/lib/x86_64-linux-gnu/libnvidia-ml.so usr/lib/x86_64-linux-gnu/libnvidia-ml.so none bind,optional,create=file
lxc.mount.entry: /usr/lib/x86_64-linux-gnu/libnvidia-ml.so.1 usr/lib/x86_64-linux-gnu/libnvidia-ml.so.1 none bind,optional,create=file
```
## NVIDIA GPU Passthrough Setup for LXC (Proxmox VE)

To map the NVIDIA GPU devices to the unprivileged container, add each of the following device nodes via the Proxmox web interface (or directly in the container configuration file):

### Device Nodes to Add

1. `/dev/nvidia0`
2. `/dev/nvidiactl`
3. `/dev/nvidia-uvm`
4. `/dev/nvidia-uvm-tools`

---

### Configuration Parameters per Device

When adding each of the 4 devices in the container's resource settings, use the following values:

| Field | Value |
| :--- | :--- |
| **Device Path** | *(One of the device paths listed above)* |
| **UID in CT** | `0` |
| **GID in CT** | `0` |
| **Access Mode in CT** | `0666` |
| **Read Only** | Unchecked |
| **Advanced** | Checked |
---

> **Permissions Note:** Setting the mode to `0666` ensures that internal container processes (such as Ollama or Docker) can interact directly with the GPU nodes without requiring special user or group privileges inside the CT.

And start LXC

test_
Test nvidia drivers:
```bash
nvidia-smi && lsmod | grep nvidia && dkms status
 ```
```conf
+-----------------------------------------------------------------------------------------+
| NVIDIA-SMI 580.196.07             Driver Version: 580.196.07     CUDA Version: N/A      |
+-----------------------------------------+------------------------+----------------------+
| GPU  Name                 Persistence-M | Bus-Id          Disp.A | Volatile Uncorr. ECC |
| Fan  Temp   Perf          Pwr:Usage/Cap |           Memory-Usage | GPU-Util  Compute M. |
|                                         |                        |               MIG M. |
|=========================================+========================+======================|
|   0  NVIDIA GeForce GTX 970         Off |   00000000:85:00.0 Off |                  N/A |
| 71%   36C    P0             42W /  250W |       0MiB /   4096MiB |      0%      Default |
|                                         |                        |                  N/A |
+-----------------------------------------+------------------------+----------------------+

+-----------------------------------------------------------------------------------------+
| Processes:                                                                              |
|  GPU   GI   CI              PID   Type   Process name                        GPU Memory |
|        ID   ID                                                               Usage      |
|=========================================================================================|
|  No running processes found                                                             |
+-----------------------------------------------------------------------------------------+
nvidia_uvm           1986560  0
nvidia_drm            131072  0
nvidia_modeset       1687552  1 nvidia_drm
nvidia              105353216  3 nvidia_uvm,nvidia_drm,nvidia_modeset
drm_ttm_helper         20480  1 nvidia_drm
video                  77824  1 nvidia_modeset
```
