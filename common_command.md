#### Query Command   
keystone user-list         查询用户信息    
keystone role-list         查询角色信息    
keystone tenant-list       查询租户信息    
glance index               查询当前存在的镜像信息    
nova image-list            查看当前存在的镜像状态    
nova secgroup-list         查看当前存在的安全组    
nova keypair-list          查看当前存在的密钥    
nova flavor-list           查看当前可以创建的实例类型    
nova list                  查看实例的状态    
nova console-log cirros    查看实例cirros的启动日志信息    
nova-manage service list   查询当前启动的Compute服务状态    
nova-manage version        查询当前安装软件的版本    
nova-manage vm list        列出所有的实例状态   
nova-manage fixed list     列出所有的固定内网IP地址信息及分配情况       
nova-manage floating list  列出所以浮动IP地址信息及分配情况    
nova-manage host list      列出当前主机的信息     
nova-manage network list   列出当前网络的相关信息    
nova-manage logs errors    列出错误的日志信息    
nova-manage logs syslog    列出syslog日志信息  

nova flavor-list    
nova show myserver1    
nova console-log myserver2    
nova delete myserver2    

#### Vistual Command
brctl    
virsh    

tgtadm --lld iscsi --op show --mode target 控制节点查看target    
    tgtadm --op new --lld=iscsi --mode=target --tid=1 --targetname=iqn.2010-10.org.openstack:volume-00000001 

nova 挂接实例/分区    
    nova-rootwrap guestmount --rw -a /home/instances/instance-0000001b/disk -m /dev/sda1 /tmp/tmpfe4fNg
