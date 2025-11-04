# truenas-intel-gpu-dkms-ko
i915 or xe sriov dkms prebuilt module
This repository provides a prebuilt ko file of **i915-sriov-dkms** for truenas scale.  

## Warning
This package is highly experimental, you should only use it when you know what you are doing.

## Usage 
Download the kernel ko file 

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
modinfo xe | grep -i filename
```

And just reboot
```
reboot
```


Built from unmodified upstream source:
- Upstream: https://github.com/intel-gpu/i915-sriov-dkms  
- Version: 2025.10.10  
- License: GPL-2.0-only  
- Kernel: 6.12.33-production+truenas  
- Compiler: gcc 13.2.0  


The module is distributed under the terms of the GNU General Public License v2.  
See [LICENSE](./LICENSE) for details.
