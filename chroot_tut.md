## CHROOT tutorial:  
  1. [Intro] (#intro)   
  2. [Commands] (#cmd)  
  3. [VirtualBox Live Cd](#live)  
  4. []()
  
<a id = "intro"> </a>
### Intro: 
After changing the file format of `/dev/vdb` to 'xfs ' in `/etc/fstab` the Magellan instances
no longer boot.   
This tutorial was created in order to recover those instances (without having to start from scratch).  
The instances are being used to run a Greenplum database.   

_The crash_:
`fstab` was modified, specifically `/dev/vdb` was set to `xfs` instead of `auto` (among other changes).  
This was done for performance reasons and in accordance with the Greenplum documentation.  
Initially, after changing the fstab configuration, unmounting `/dev/vdb`, formatting `vdb` to xfs, and then remounting `/dev/vdb`
the system was able to reboot.  
  
However, it became necessary to rebuild the nodes using earlier snapshots. Upon rebuilding and rebooting the machines, 
the system crashed, since it was unable to mount the drives as `xfs`.   

Our tutorial will dwelve into using `chroot` to restore fstab in order to get the image running again.  

<a id ="live"></a>
### VirtualBox Live:  
For this example, we install Ubuntu 16.04. We modify the `/etc/fstab` to make it crash.  
Modify the fstab file to look like this:   

  ![crash_ubuntu]()  

Attempt to reboot. Ubuntu should crash.  

Now, to start fixing we need to boot onto a live cd. Be sure to have the original image (iso)  
available. 

On VirtualBox change the boot order, so that **`CD/DVD-ROM`** or **`Optical`** is first:  
 ![boot_order](https://github.com/syuja/ssh_tut/blob/master/img/boot_order.png)  

Start the Virtual Machine:  
  ![start_machine](https://github.com/syuja/ssh_tut/blob/master/img/start_machine.png)  
  
On startup, press `Host` + `P`. The `Host` key is by default the right control key.  
This should pause the startup process. Now click on `Devices`, and select the original Ubuntu  
image. Unpause the start up by pressing `Host` + `P` again.   
  
  ![ubuntu_image](https://github.com/syuja/ssh_tut/blob/master/img/boot_order.png)  
  
To verify the the cd has been mounted:  
  ![verify](https://github.com/syuja/ssh_tut/blob/master/img/verify.png)
When the installation screen show up, click on `Try Ubuntu`.   
  ![try_ubuntu](https://github.com/syuja/ssh_tut/blob/master/img/try_ubuntu.png)  
  
At this point you should have a booted live Ubuntu cd.  


<a id = "cmd"> </a>
## Commands:  
`chroot` - run a command or interactive shell with special root directory  

     #syntax:  
     #chroot <option> <newroot_file> <command>  
     #example:  
     chroot /mnt/mydrive /bin/bash  
  
  
mkdir -p <--- -p flag creates missing directories if they're not there.
mount -bind
