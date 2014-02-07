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

#### configure netbrige
	
	brctl addbr br100
	brctl stp br100 on
	brctl addif br100 eth1
	brctl addif br100 **vm interface**
	ifconfig br100 10.0.0.1 netmask 255.255.255.0 up    
	
if can't ping access    

	[root@nsn-controller ~]# ping 10.0.0.11
	PING 10.0.0.11 (10.0.0.11) 56(84) bytes of data.
	From 10.0.0.1 icmp_seq=1 Destination Host Unreachable
	From 10.0.0.1 icmp_seq=2 Destination Host Unreachable    

delete interface from bridge and add again.

	
#### start nova-network(may be not needed)

	vim /opt/tcp/tcp_config
	add "nova-network"
	
#### disabled route for eth1

	route del -net 10.0.0.0 netmask 255.255.255.0 dev eth1
	
	
## Q&A    
* Error：AMQP server on 10.0.0.23:5672 is unreachable: [Errno 111] ECONNREFUSED. Trying again in 3 seconds.

    	service rabbitmq-server restart


* Some usefull init file.    

    	[root@nsn-controller /tmp]# find / -name "api-paste.ini"    
    	/etc/neutron/api-paste.ini   
    	/etc/cinder/api-paste.ini    
    	/etc/nova/api-paste.ini    
    	/etc/heat/api-paste.ini   
    	/usr/etc/neutron/api-paste.ini      

* Enable & Disabled Debug   

    	/etc/nova/nova.conf 
		    ---> debug=false(true)

		[root@nsn-controller /tmp]# nova-manage service list
		Binary           Host                                 Zone             Status     State Updated_At
		nova-consoleauth nsn-controller                       internal         enabled    :-)   2014-01-27 13:58:50
		nova-scheduler   nsn-controller                       internal         enabled    :-)   2014-01-27 13:58:51
		nova-conductor   nsn-controller                       internal         enabled    :-)   2014-01-27 13:58:52
		nova-compute     nsn-controller                       nova             enabled    :-)   2014-01-27 13:58:52
		nova-network     nsn-controller                       internal         enabled    XXX   2014-01-24 00:17:53
		nova-cert        nsn-controller                       internal         enabled    XXX   2014-01-24 00:17:53
