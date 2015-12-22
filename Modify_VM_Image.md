#Modify VM images
###Contents
Project: [http://libguestfs.org/](http://libguestfs.org/ "libguestfs")   
Modify images    
&nbsp;&nbsp;guestfish Example guestfish session   
&nbsp;&nbsp;Go further with guestfish   

guestmount
&nbsp;&nbsp;virt-* tools    
&nbsp;&nbsp;Modify a single file inside of an image   
&nbsp;&nbsp;Resize an image   

Loop devices, kpartx, network block devices   
&nbsp;&nbsp;Mount a raw image (without LVM)   
&nbsp;&nbsp;Mount a raw image (with LVM)   
&nbsp;&nbsp;Mount a qcow2 image (without LVM)   
&nbsp;&nbsp;Mount a qcow2 image (with LVM)   

Reference Link:
[http://docs.openstack.org/image-guide/modify-images.html](http://docs.openstack.org/image-guide/modify-images.html)



###Mount a qcow2 image (without LVM)

You need the nbd (network block device) kernel module loaded to mount qcow2 images. This will load it with support for 16 block devices, which is fine for our purposes. As root:

    
    # modprobe nbd max_part=16
    
    
Assuming the first block device (/dev/nbd0) is not currently in use, we can expose the disk partitions using the qemu-nbd and partprobe commands. As root:

    
    # qemu-nbd -c /dev/nbd0 image.qcow2
    # partprobe /dev/nbd0
    
    
If the image has, say three partitions (/boot, /, swap), there should be one new device created for each partition:
    
    
    $ ls -l /dev/nbd3*
    brw-rw---- 1 root disk 43, 48 2012-03-05 15:32 /dev/nbd0
    brw-rw---- 1 root disk 43, 49 2012-03-05 15:32 /dev/nbd0p1
    brw-rw---- 1 root disk 43, 50 2012-03-05 15:32 /dev/nbd0p2
    brw-rw---- 1 root disk 43, 51 2012-03-05 15:32 /dev/nbd0p3
    


>  Note
>  
> If the network block device you selected was already in use, the initial qemu-nbd command will fail silently, and the /dev/nbd3p{1,2,3} device files will not be created.


If the image partitions are not managed with LVM, they can be mounted directly:


    # mkdir /mnt/image
    # mount /dev/nbd3p2 /mnt/image
    
    
When you are done, clean up:

    
    # umount /mnt/image
    # rmdir /mnt/image
    # qemu-nbd -d /dev/nbd0
    


###Mount a qcow2 image (with LVM)

If the image partitions are managed with LVM, after you use qemu-nbd and partprobe, you must use vgscan and vgchange -ay in order to expose the LVM partitions as devices that can be mounted:


    # modprobe nbd max_part=16
    # qemu-nbd -c /dev/nbd0 image.qcow2
    # partprobe /dev/nbd0
    # vgscan  

Reading all physical volumes. This may take a while...
Found volume group "vg_rhel62x8664" using metadata type lvm2  

    # vgchange -ay  

2 logical volume(s) in volume group "vg_rhel62x8664" now active  

    # mount /dev/vg_rhel62x8664/lv_root /mnt


When you are done, clean up:


    # umount /mnt
    # vgchange -an vg_rhel62x8664
    # qemu-nbd -d /dev/nbd0
      


