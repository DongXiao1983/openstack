
Create PXE-Boot image for Openstack

Openstack doesn’t support launching a diskless PXE-boot VM, we have to make an image with PXE-boot capability to achieve this.

1.Create a small empty disk file, create dos filesystem.

dd if=/dev/zero of=pxeboot.img bs=1M count=4
mkdosfs pxeboot.img

2.Make it bootable by syslinux

losetup /dev/loop0 pxeboot.img
mount /dev/loop0 /mnt
syslinux --install /dev/loop0

3.Install iPXE kernel and make sysliux.cfg to load it at bootup

wget http://boot.ipxe.org/ipxe.iso
mount -o loop ipxe.iso /media
cp /media/ipxe.krn /mnt
cat > /mnt/syslinux.cfg <<EOF
DEFAULT ipxe
LABEL ipxe
 KERNEL ipxe.krn
EOF
umount /media/
umount /mnt

4.Now we have pxeboot.img ready, let’s register it to glance

source nceprc
glance image-create --name NG-AS --is-public true  --disk-format raw --container-format bare < pxeboot.img
About these ads
