LVM的优点    
● 灵活的LV
由于LV是由一个或多个LP组成,LP与PP相对应,这种对应关系被LVDD管理.LV可以不占用连续的物理硬盘空间这样LV在一个VG中可以跨硬盘存在,它的大小可以被动态增加,可以很容易的加镜像,可以很容易的被删除.

● 高可靠性

LVM通过镜像来提高数据的可靠性,被镜像的LV在系统中可以有2到3份拷贝.当一份数据被毁坏后系统可以用该数据的镜像.

● 高性能

LVM通过strping来提高系统访问数据的速度.strping技术将LV的数据分布到不同PV上访问这些数据时可以进行并行的读写.我们在创建LV时可以指定LV在PV上的分布位置,我们将经常被访问的LV放在PV的热点位置可以提高访问速度.

● 写校验

LVM可以通过写校验对每次的磁盘写操作都被校验,提供数据的稳定性.

● 动态管理

我们可以在系统正常运行期间对系统的LV进行各种操作,操作后不需要重新启动机器.这些操作大部分对用户是透明的.

● 容易使用

我们可通过使用高级命令来管理系统存储也可以通过smit来管理系统存储.    

--------    
1.fdisk 格式化磁盘    

2.创建物理卷PV:     
pvcreate /dev/sdb5 /dev/sdb6 /dev/sdb7 && pvdisplay     

3.创建卷组VG:     
vgcreate myvg1 /dev/sdb5 /dev/sdb6 /dev/sdb7 && vgdisplay  

4.创建逻辑卷LV，指定LV大小：    
lvcreate -L 100M -n mylv1 myvg1 && lvdisplay     

5.格式化LV：    
mkfs.ext4 /dev/myvg1/mylv1     
格式化的时候可以加-L参数指定下LABEL方便挂载，省去找设备路径的麻烦。    

6.挂载：    
mkdir /mnt/mylv1 && mount /dev/myvg1/mylv1 /mnt/mylv1     

7.开机挂载：    
vim /etc/fstab     
/dev/myvg1/mylv1 /mnt/mylv1 ext4 defaults 0 0    

8.LV扩容：    
lvextend -L +100M /dev/myvg1/mylv1 && resize2fs /dev/myvg1/mylv1     
    在VG还有足够的容量就使用上面的命令。执行完lvextend后，lvdisplay会立即显示扩容后的逻辑卷的大小，但实际上还分配空间，是用df -h命令查看时，就会发现还是显示原来的大小，这时执行resize2fs命令就分配了空间了，再执行df -h就看到扩容后的大小了。注意lvextend和resize2fs后面都是接的LV的设备路径。其实lvextend可以再加一个-r的参数，就不用resize2fs了，上面的命令可以简写为：    
lvextend -rL +100M /dev/myvg1/mylv1     
参数合在一起只能是-rL不能是-Lr，否则报错，因为-L后面还要接大小。    

9.VG扩容：    
vgextend myvg1 /dev/sdb8 && vgdisplay     
这是VG容量不足了，所以先扩容VG。上面命令的意思就是把分区/dev/sda8加入到卷组myvg1里去，所以VG就扩容了，接下来就扩容LV咯。    

10.LV缩小：    
umout /mnt/mylv1    
e2fsck -f /dev/myvg1/mylv1    
lvreduce -L -50M /dev/myvg1/mylv1    
mount -a     
（-50M表示总容量减少50M,如果不加减号“-”，就表示减少到50M。该操作一般很少使用，请谨慎执行）    
等效命令：    
lvresize -rL 100M /dev/myvg1/mylv1    
 
