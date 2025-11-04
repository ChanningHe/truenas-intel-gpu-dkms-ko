# truenas-intel-gpu-dkms-ko
i915 or xe sriov dkms prebuilt module
This repository provides a prebuilt ko file of **i915-sriov-dkms** for truenas scale.  

## Warning
This package is highly experimental, you should only use it when you know what you are doing.

## Usage 
Please refer to the upstream repository for the purpose and usage of each .ko module.

In my case, I’m using an Intel Arc B50 on Proxmox VE with the 6.17 mainline kernel to enable SR-IOV, and then passing the VF through to TrueNAS SCALE 25.10.
This setup is necessary because the 6.12 “XE” kernel driver in TrueNAS SCALE 25.10 cannot drive the B50’s VF properly.

### Below is my Usage section:
Download the kernel ko file 
```
KOVERSION=20251010
mkdir ./kofiles

wget -O ./kofiles/xe.ko https://github.com/ChanningHe/truenas-intel-gpu-dkms-ko/raw/refs/heads/main/kofiles-${KOVERSION}/xe.ko

wget -O ./kofiles/intel_sriov_compat.ko https://github.com/ChanningHe/truenas-intel-gpu-dkms-ko/raw/refs/heads/main/kofiles-${KOVERSION}/intel_sriov_compat.ko
```
Disable readonly
```
zfs set readonly=off boot-pool/ROOT/25.10.0/usr
```

Copy the 
```
mkdir -p /lib/modules/$(uname -r)/updates/dkms/
cp ./kofiles/*.ko /lib/modules/$(uname -r)/updates/dkms/
```

depend all module
```
depmod -a
```

Make sure your ko file is under the /lib/modules/$(uname -r)/updates/dkms/
```
root@truenas[~]# modinfo xe | grep -i filename
filename:       /lib/modules/6.12.33-production+truenas/updates/dkms/xe.ko
```

And just reboot
```
reboot
```

In my case, the kernel shows some error messages,
but as long as /dev/dri/renderD128 appears, it means the driver has been successfully initialized.
```
root@truenas[~]# ls /dev/dri 
by-path  card0  card1  renderD128
```

---

Built from unmodified upstream source:
- Upstream: https://github.com/intel-gpu/i915-sriov-dkms  
- Version: 2025.10.10  
- License: GPL-2.0-only  
- Kernel: 6.12.33-production+truenas  
- Compiler: gcc 13.2.0  


The module is distributed under the terms of the GNU General Public License v2.  
See [LICENSE](./LICENSE) for details.
