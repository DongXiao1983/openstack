#### 1. Query Command   
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

    [root@nsn-controller ~]# nova list
    +--------------------------------------+-----------------+--------+------------+
    | ID                                   | Name            | Status | Task State |
    +--------------------------------------+-----------------+--------+------------+
    | 85fd31e4-7230-45b0-817b-7f0556ecfbf0 | Test            | ACTIVE | None       |
    | e0d06eaa-7886-4530-9cb9-b5d90a7df266 | test_instance-1 | ACTIVE | None       |
    | 38aca0b8-6294-4b3f-ba10-b0872d2696af | test_instance-2 | ACTIVE | None       |
    | c9a64170-9768-42da-a83e-653ff914dc78 | tt              | ACTIVE | None       |
    +--------------------------------------+-----------------+--------+------------+

nova show *Test*    

    [root@nsn-controller ~]# nova show Test
    +--------------------------------------+----------------------------------------------------------+
    | Property                             | Value                                                    |
    +--------------------------------------+----------------------------------------------------------+
    | status                               | ACTIVE                                                   |
    | updated                              | 2014-01-24T20:35:49Z                                     |
    | OS-EXT-STS:task_state                | None                                                     |
    | OS-EXT-SRV-ATTR:host                 | nsn-controller                                           |
    | key_name                             | openstack                                                |
    | image                                | ubunto (b9994658-61c6-4a59-b27e-f6f9c3de9f21)            |
    | test_network network                 | 10.0.0.6                                                 |
    | hostId                               | 6c9deb7b822076ea65ad7fed678caf877b9201f40fb76cfb0d1f59ea |
    | OS-EXT-STS:vm_state                  | active                                                   |
    | OS-EXT-SRV-ATTR:instance_name        | instance-0000000c                                        |
    | OS-SRV-USG:launched_at               | 2014-01-24T20:35:49.000000                               |
    | OS-EXT-SRV-ATTR:hypervisor_hostname  | nsn-controller                                           |
    | flavor                               | m1.small (2)                                             |
    | id                                   | 85fd31e4-7230-45b0-817b-7f0556ecfbf0                     |
    | security_groups                      | [{u'name': u'default'}]                                  |
    | OS-SRV-USG:terminated_at             | None                                                     |
    | user_id                              | f093c86ccae54a259afd7451bea413c8                         |
    | name                                 | Test                                                     |
    | created                              | 2014-01-24T20:34:44Z                                     |
    | tenant_id                            | 447271ce0ce44ad2ae0b2a26fb35b215                         |
    | OS-DCF:diskConfig                    | MANUAL                                                   |
    | metadata                             | {}                                                       |
    | os-extended-volumes:volumes_attached | []                                                       |
    | accessIPv4                           |                                                          |
    | accessIPv6                           |                                                          |
    | progress                             | 0                                                        |
    | OS-EXT-STS:power_state               | 1                                                        |
    | OS-EXT-AZ:availability_zone          | nova                                                     |
    | config_drive                         |                                                          |
    +--------------------------------------+----------------------------------------------------------+

nova console-log *Test*    
nova delete *Test*    

#### 2. Vistual Command
brctl    
virsh    
    
tgtadm --lld iscsi --op show --mode target 控制节点查看target    

    tgtadm --op new --lld=iscsi --mode=target --tid=1 --targetname=iqn.2010-10.org.openstack:volume-00000001 

nova 挂接实例/分区    

    nova-rootwrap guestmount --rw -a /home/instances/instance-0000001b/disk -m /dev/sda1 /tmp/tmpfe4fNg

#### 3. 重新初始化nova库：    
停止控制和计算节点的nova服务,删除/home/instances目录下的文件    

    nova-manage network delete 192.168.193.0/24
    nova-manage network delete 10.18.4.0/24
    mysql -uroot -p
    mysql> drop database nova
    mysql> create database nova;
    mysql> grant all privileges on nova.* to 'nova_user'@'%' identified by 'nova_pw3465';
    nova-manage db sync
    nohup /usr/bin/python /usr/bin/nova-all >> /var/log/nova.log 2>&1 &
    nova-manage network create --label=public --fixed_range_v4=10.18.4.0/24 --num_networks=1 --network_size=256 --gateway=10.18.4.254 --bridge=br_pu --bridge_interface=em1 --multi_host='F'
    nova-manage network create --label=private --fixed_range_v4=192.168.193.0/24 --num_networks=1 --network_size=256 --gateway=192.168.193.1 --bridge=br_pr --bridge_interface=em2 --multi_host='F'
    
创建密钥：    

    nova keypair-add mykey > oskey.priv
    chmod 600 oskey.priv
    
设置安全策略 

    nova secgroup-add-rule default tcp 22 22 0.0.0.0/0
    nova secgroup-add-rule default icmp -1 -1 0.0.0.0/0
    nova secgroup-list
    nova secgroup-list-rules default
    
上传镜像

    glance add name="CentOS-6.3_Fina_qcow2" is_public=true container_format=bare disk_format=qcow2 < CentOS-6.3_Fina_qcow2.img
    glance add name="CentOS-6.3_Fina_raw" is_public=true container_format=bare disk_format=raw < CentOS-6.3_Fina_raw.img

重新生成实例


#### 4. 绑定KVM的cpu到主机固定CPU    

    ps -eL | grep kvm    
    cat /proc/xxxx/statu    
    Cpus_allowed: 3    
    Cpus_allowed_list: 0-1    
    
    
