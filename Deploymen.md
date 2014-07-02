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

May 21st, 2014 | Comments

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
