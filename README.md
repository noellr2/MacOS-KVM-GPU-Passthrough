# This is a fork of foxlet/macOS-Simple-KVM, so make sure to check them out!
This is an automatic installer for making a MacOS VM with GPU passthrough.

# Preperations
To prepare, make sure you have virtualization enabled in your BIOS.

For chad AMD this could be done by enabling

• IOMMU • NX Mode • SVM Mode

For you lame Intel users, just enable

• VT-d • VT-x

And then clone the repository by typing:
To start, make sure to clone the repository in the folder you want your VM to be stored.
```
git clone https://github.com/BigAnteater/KVM-MacOS-GPU-Passthrough && cd KVM-MacOS-GPU-Passthrough
```

# Setting up GRUB

if you have already set your bootloader up, you can skip this step but this is for the people who are doing this for the first time.

Preparing GRUB is very simple. Just follow these instructions.

1. Mark the script as executable: for AMD: `chmod +x grub_setup_amd.sh` for Intel: `chmod +x grub_setup_intel.sh.`
3. Then run the script: AMD: `sudo ./grub_setup_amd.sh` Intel: `sudo ./grub_setup_intel.sh.`
4. Then just follow the instructions in script!
5. Once you finish, reboot.

# Configuring Libvirt + other applications

To configure libvirt run my script which configures libvirt and QEMU for you.
Mark the script as executable
```
chmod +x libvirt_configuration.sh
```

Then run the script
```
sudo ./libvirt_configuration.sh
```
**Note: if it asks you to replace `iptables` with `iptables-nft`, just say yes.**

# Creating the VM

To make this very simple to set up and install, I created a script that installs the VM for you.
To run it, do the following:
```
chmod +x setup.sh
```
Then once it's executable, run the script.
```
./setup.sh
```
The script will walk you through making the VM.

**NOTE: if you have an older nvidia card (most cards that came before the 1080 ti), you might be able to run high sierra with GPU acceleration.**

Once you have made the VM, it will show up in virtual machine manager. You can edit the RAM settings and the CPU configuration. I reccomend settings the CPU mode to host-passthrough.

**NOTE: the VM will only boot if you give it either 2, 4, or 8 cores.**

Lastly, make sure the other two drives are bootable.

![Screen Capture_select-area_20220129125925](https://user-images.githubusercontent.com/77298458/151677424-8d6492b7-5509-4a9f-a52e-60291d3f7e38.png)

# Setting up the passthrough hooks

To set up the passthrough hooks, we will be using a QEMU hooks script made by RisingPrismTV.
To install it, we will want to type the following commands:
```
git clone https://gitlab.com/risingprismtv/single-gpu-passthrough/ && cd single-gpu-passthrough
```
Then we will be running the install script:
```
sudo ./install_hooks.sh
```
Now your hooks should be good to go!

# Adding the GPU to the VM

**Note: I'm skipping patching the ROM just because AMD graphics cards don't need them. If you happen to be using a compatible NVIDIA card, I've linked a timestamp to my video where I showcase how to do so. https://youtu.be/R5c25kV4tZ0?t=306**

Adding the GPU is very simple.

1. Open your VM in virt-manager.
2. Cick add hardware and choose PCI host device.
![Screen Capture_select-area_20220129132902](https://user-images.githubusercontent.com/77298458/151678134-8cdde865-d85a-437a-9bf1-3b5c74b224a3.png)
3. Choose your everything related to your graphics card. For AMD GPU users, if your GPU and GPU audio device are in different groups, that's fine.
4. Finally, add your USB controller, and your audio controller.

# Closing notes

Now you should be able to power on your VM, and use your GPU with it!
