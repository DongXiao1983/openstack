1. Install CentOS 6.5    

    These parts is needed:   
		 
		gcc
		virtualization
		python > 3.0
		vnc

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
