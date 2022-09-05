# Thinkpad Bluetooth Keyboard II - Driver patch for Linux 5.15

Issue:

- The middle button always pasted the clipboard, even when used for scrolling
- FN keys did not work (e.g. F4, F9, F10)

Solution:

- Use the 5.19 linux kernel keyboard files to install the patch yourself.

Information:

- Not needed anymore after kernel 5.19 is used on your system. Thank you for doing these updates

**Final steps to apply the (modified) patch in kernel 5.15:**

1. (already done, but you can do that yourself as well)
Go to <https://github.com/torvalds/linux/tree/v5.19/drivers/hid>
and copy the content from hid-ids.h and hid-lenovo.c into the files here, to that
the files from the new kernel are used (a stable release with first occurance of that fix).

2. Compile the files and install them to the current kernel

Compile the modified patched module with the also-modified make script:

```sh
make clean
make
sudo make modules_install
```

```sh
# make.sh doing the installation for all kernels: Not needed in my opinion. Only newest kernel is enough
# (lead to some errors for older kernels for me)
#./make.sh
```

Backup the unpatched modules

```sh
kernel=`uname -r`
cp /lib/modules/${kernel}/kernel/drivers/hid/hid-lenovo.ko hid-lenovo.ko.unpatched
```

Backup the patched modules

```sh
sudo cp /lib/modules/${kernel}/extra/hid-lenovo.ko hid-lenovo.ko.patched
```

Make patched module the default one

```sh
sudo cp -f /lib/modules/${kernel}/extra/hid-lenovo.ko /lib/modules/${kernel}/kernel/drivers/hid/hid-lenovo.ko
```

Reload the module

```sh
sudo rmmod hid-lenovo
sudo modprobe hid-lenovo
```

Make the module load automatically:

```sh
echo -e "\nhid-lenovo" | sudo tee -a /etc/modules-load.d/modules.conf
cat  /etc/modules-load.d/modules.conf
```
