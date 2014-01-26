##How to deploy VM 
#### 1. Create VM image

1.create virtul disk     
   
	qemu-img create -f qoow2 /tmp/centos-6.4.qcow2 10G

2.install os to virtul disk   
	
    virt-install --virt-type kvm --name centos-6.5 --ram 1024 \ 
    --cdrom=/tmp/CentOS-6.5-x86_64-minimal.iso \
    --disk /tmp/centos-6.4.qcow2,format=qcow2 \
    --network network=default --graphics vnc,listen=0.0.0.0 --noautoconsole \
    --os-type=linux --os-variant=rhel6
    
Use the VNC (10.56.212.18:0 ) to connect and install OS.


4.test the img, and do some config such as  SSH , firewall, etc.
	   
	    
	[root@CON-Cloud /var/log/glance]# virsh start centos-6
	Domain centos-6 started
	
	[root@CON-Cloud /var/log/glance]# virsh vncdisplay centos-6
	:2
	
	[root@CON-Cloud /var/log/glance]# virsh list --all
	 Id    Name                           State
	----------------------------------------------------
	 2     instance-00000002              running
	 3     centos-6                       running
	   

Now you can use port 2 to access the VM by VNC.

#### 2.  Publish to Openstack
	glance index   
	glance add name="centos6.5" disk_format=qcow2 is_public=true container_format=ovf < ./centos-6.5.qcow2

After publish sucessfull, to check the image .

	[root@CON-Cloud /var/log/glance]# glance index
	ID                                   Name                           Disk Format          Container Format     Size
	------------------------------------ ------------------------------ -------------------- -------------------- --------------
	d9b662ec-b91c-4ac3-9905-f1fd192306b9 centos6.5                      qcow2                ovf                      1359478784

#### 3. Enable console & log

#### 4. Login to **dashboard** 
   
	[root@CON-Cloud /var/log/glance]# virsh list --all
	 Id    Name                           State
	----------------------------------------------------
	 -     centos-6                       shut off 
	
	[root@CON-Cloud /var/log/glance]# virsh list --all
	 Id    Name                           State
	----------------------------------------------------
	 2     instance-00000002              running
	 -     centos-6                       shut off
	 


#### floating IP resource
	
	# nova-manage floating create 10.10.0.0/24
	# nova-manage floating list


	nova-manage service list

## Q&A    
* Error：AMQP server on 10.0.0.23:5672 is unreachable: [Errno 111] ECONNREFUSED. Trying again in 3 seconds.
	
	service rabbitmq-server restart
