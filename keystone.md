1. /etc/keystone/keystone.conf

	    admin_token=ADMIN
	    admin_bind_host=100.0.0.11
	    admin_port=35357
	    public_port=5000
	    
	    [auth]
	    methods=external,password,token
	    password=keystone.auth.plugins.password.Password
	    token=keystone.auth.plugins.token.Token
	    
	    [catalog]
	    driver=keystone.catalog.backends.sql.Catalog
	    
	    [database]
	    connection = mysql://keystone:keystone@100.0.0.11/keystone
	    
	    [identity]
	    driver=keystone.identity.backends.sql.Identity
	    
2. openstack-config --set /etc/keystone/keystone.conf database connection mysql://keystone:keystone@100.0.0.11/keystone    

3. Create DB

4. Create user

5. Add permission

5. Sync DB

6. export TOKEN & ENDPOINT     

7. Create roles

8. Create tenata

9. Add role

10. Create endpoint



概念：    
tenant更像是一个声明，是一个资源的集合，说它是声明，是因为具体的资源可以被多次声明包含在一个tenant中。比如Nova中虚拟机器的集合，Swift中容器的集合，glance中镜像的集合等。     

service是以service catalog的形式提供的，就是所有支持的服务的集合，并且指的是OpenStack中的服务，比如Compute Service,Image Service。在keystone中是声明，具体服务由特定OpenStack组件实现。    
 
endpoint应该是附属于service的数据模型，是服务访问点，通常是一个URL，包括ip地址，端口号等。利用这些endpoint可以实现RESTful方式访问OpenStack服务，通过URI调用OpenStack API       
