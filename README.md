# amd-dkms-patch
Custom `dkms.conf` file for installing amdgpu on Ubuntu kernel ≥ 6.8.0-44

## Context

When Ubuntu released kernel 6.8.0-44, installation of `amdgpu-dkms` from https://repo.radeon.com/amdgpu/6.2/ubuntu ceased to work.

For the whole story, see https://github.com/ROCm/ROCm/issues/3701

## Solution

The prototype of function `drm_dp_update_payload_part2()` was changed and a custom dkms.conf can make the installation work again.

After the installation have failed, put the file `amdgpu-6.8.5-2009582.24.04.conf` in `/etc/dkms`. The version number matches the one of the module we need to build, so it will supersede the one donwloaded from radeon.com.

Then:

~~~ 
sudo dkms build amdgpu/6.8.5-2009582.24.04 -k 6.8.0-44-generic
sudo dkms install amdgpu/6.8.5-2009582.24.04 -k 6.8.0-44-generic
sudo update-initramfs -c -k 6.8.0-44-generic
~~~


## Limitations

It was tested on my Ubuntu 24.04 with kernel 6.8.0-44. I am sharing this hopping it could help others.

The only guarantee is that it worked for me. 
