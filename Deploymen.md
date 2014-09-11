1. Install CentOS 6.5    

    These parts is needed:   
    	 
		gcc
		virtualization
		python > 3.0
		vnc
		xfsprogs   
		xinetd
		mongodb-server
		mongodb
		lzip
		gmp > 6.0

2. Make a icehouse repo   

		[openstack-icehouse]
		name=OpenStack Icehouse Repository
		baseurl=http://repos.fedorapeople.org/repos/openstack/openstack-icehouse/epel-6/
		enabled=1
		skip_if_unavailable=0
		gpgcheck=1 
		gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-RDO-Icehouse   
		priority=98    

3. Make a commom repo    

		[common-rpm]   
		name=Puppet Labs Products - $basearch   
		baseurl=http://dl.fedoraproject.org/pub/epel/6/x86_64    
		enabled=1
		gpgcheck=0    

4. Make Ruby repo    

		[scl_ruby193]
		name=Ruby 1.9.3 Dynamic Software Collection
		baseurl=http://people.redhat.com/bkabrda/ruby193-rhel-6/
		failovermethod=priority
		enabled=1
		gpgcheck=0
   

5. Install python 3.0+     

   download, and make install
   
6. Install openstack rpms    
    
	    yum install openstack-swift*  
        yum install openstack-nova* 
	    yum install openstack-glance*    
	    yum install openstack-horizen*    
	    yum install openstack-hori*    
	    yum install openstack-utils    
	    yum install openstack-selinux     
	    yum install openstack-keystone python-keystoneclient    
	    yum install openstack-dashboard*    
	    yum install openstack-cinder*    
	    yum install rabbitmq-server*    
	    yum install libvirt*    
	    yum install openstack-heat-*    
	    yum install openstack-ceilometer*    
	    yum install openstack-trove* 

add: openstack-nova maybe has conflicate between icehouse and common repo


7. Env export:    

        [root@cloud ~]export SERVICE_TOKEN="ADMIN"
        [root@cloud ~]export SERVICE_ENDPOINT="http://100.0.0.11:35357/v2.0"
        [root@cloud ~]unset OS_TENANT_NAME
        [root@cloud ~]unset OS_USERNAME
        [root@cloud ~]unset OS_PASSWORD
        [root@cloud ~]unset OS_AUTH_URL
        [root@cloud ~]keystone user-list
        +----------------------------------+--------+---------+-------------------+
        |                id                |  name  | enabled |       email       |
        +----------------------------------+--------+---------+-------------------+
        | c6bdf1c356ef40eb80e7926b9c129b6b | admin  |   True  |  admin@domain.com |
        | fac91d6cf3454fd584c7308e2618cb8c | glance |   True  | glance@domain.com |
        +----------------------------------+--------+---------+-------------------+
	
8. Dashboard    

        vim /etc/sysconfig/memcached    
        
        PORT="11211"
        USER="memcached"
        MAXCONN="8192"
        CACHESIZE="122445"
        OPTIONS="-l 0.0.0.0 -U 11211 -t 32"
	
9. keystone middleware

Identity_uri in Auth Token Middleware    
   

   As part of the 0.8 release of keystoneclient (2014-04-17) we made an update to the way that you configure auth_token middleware in OpenStack.    

   Previously you specify the path to the keystone server as a number of individual parameters such as:    

        [keystone_authtoken]
        auth_protocol = http
        auth_port = 35357
        auth_host = 127.0.0.1
        auth_admin_prefix =


This made sense in code when using httplib for communication where you use each of those independent pieces. However we removed httplib a number of releases ago and now simply reconstruct the full URL in code in the form:    

        %(auth_protocol)s://%(auth_host)s:%(auth_port)d/%(auth_admin_prefix)s

    This format is much more intuitive for configuration and so should now be used with the key identity_uri. e.g.    

        [keystone_authtoken]
        identity_uri = http://127.0.0.1:35357

   Using the original format will continue to work but you’ll see a deprecation message like:    

   WARNING keystoneclient.middleware.auth_token [-] Configuring admin URI using auth fragments. This is dep    





10.OpennStack Install instance    

    glance image-create --name SPRIENT --disk-format=raw --is-public=true --container-format=bare < /root/vCFPU0.img


    neutron port-create internel --mac-address 52:54:00:12:CF:01
    neutron port-create internel --mac-address 52:54:00:12:CF:02
    neutron port-create internel --mac-address 52:54:00:12:CF:03
    neutron port-create internel --mac-address 52:54:00:12:CF:04
    neutron port-create internel --mac-address 52:54:00:12:CF:05
    neutron port-create externel --mac-address 52:54:00:de:ad:e1
    neutron port-create externel --mac-address 52:54:00:de:ad:e2
    neutron port-create externel --mac-address 52:54:00:de:ad:e3
    neutron port-create externel --mac-address 52:54:00:de:ad:e4



        nova boot --flavor CFPU --image `glance image-list | grep cfpu | awk ' { print $2 }'`  --nic port-id=`neutron port-list | grep 12:CF:01 | awk ' { print $2 }'` --nic port-id=`neutron port-list | grep ad:e1 | awk ' { print $2 }'` CFPU
        nova boot --flavor CSPU --image ec2c535d-7cca-4e79-901e-02fec18526aa --nic port-id=`neutron port-list | grep 12:CF:02 | awk ' { print $2 }'` --nic port-id=`neutron port-list | grep ad:e2 | awk ' { print $2 }'` CSPU
        nova boot --flavor USPU --image ec2c535d-7cca-4e79-901e-02fec18526aa --nic port-id=`neutron port-list | grep 12:CF:03 | awk ' { print $2 }'`  --nic port-id=`neutron port-list | grep ad:e3 | awk ' { print $2 }'` USPU
        
        nova boot --flavor EIPU --image ec2c535d-7cca-4e79-901e-02fec18526aa --nic  port-id=`neutron port-list | grep 12:CF:04 | awk ' { print $2 }' ` --nic port-id=`neutron port-list | grep ad:e4 | awk ' { print $2 }' `  EIPU 



        nova interface-attach --port-id `neutron port-list | grep ad:e5 | awk ' { print $2 }' `  `nova list | grep -i eipu |  awk ' { print $2 }' `
        nova interface-attach --port-id `neutron port-list | grep CF:05 | awk ' { print $2 }' `  `nova list | grep -i eipu |  awk ' { print $2 }' `



        ovs-vsctl list Interface | grep ofport_request -B 3    
        
10.    Pack    

        1 yum update
        2  ifconfig
        3  shutdown
        4  init 0
        5  ll
        6  yum install yum-plugin-priorities
        7  yum install http://repos.fedorapeople.org/repos/openstack/openstack-icehouse/rdo-release-icehouse-3.noarch.rpm
        8  yum install http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
        9  ps -ef | grep yum
        10  yum install http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
        11  yum install openstack-utils
        12  yum install openstack-selinux
        13  yum upgrade
        14  yum install -y openstack-packstack
        15  ifconfig eth0
        16  ifconfig eth1
        17  yum install wget -y
        18  fdisk
        19  fdisk -l
        20  packstack --install-hosts=192.168.1.104
        21  yum -d 0 -e 0 -y install mariadb-galera-server
        22  yum --help
        23  yum erase mysql-server-5.1.73-3.el6_5.x86_64
        24  packstack --install-hosts=192.168.1.104
        25  yum install restorecon*
        26  yum install *restorecon*
        27  yum -y install policycoreutils
        28  vim /var/tmp/packstack/20140703-050807-TC189x/manifests/192.168.1.104_swift.pp.log
        29  cd /etc/glance/glance-api.conf
        30  openstack-status
        31  ll
        32  vim keystonerc_admin
        33  vim packstack-answers-20140703-050807.txt
        34  vim /etc/glance/glance-api.conf
        35  service openstack-glance-api status
        36  openstack-status
        37  ll
        38  source keystonerc_admin
        39  openstack-status
        40  exit
        41  init 0
        42  ll
        43  history
        44  q!

