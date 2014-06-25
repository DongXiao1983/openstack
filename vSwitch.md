openvswitch

libvirt版本是目前最新的libvirt 0.9.13.

教程会分为一下几步:

编译, 安装libvirt 0.9.13    
编译, 安装openvswitch 1.6.1    
启动openvswitch     
创建openvswitch网桥, 划分子网     
启动vm到指定网桥     

1. 编译, 安装libvirt 0.9.13   
2.  
		$ ./autogen.sh --system 
		$ make
		$ sudo make install

注意1: 在执行 autogen.sh 脚本是时候, 添加上的 --system , 是为了让编译时候指定到对应的系统文件夹.     
可以发现其实是执行了一下的命令 ./configure with --prefix=/usr --sysconfdir=/etc --localstatedir=/var --libdir=/usr/lib , 上面的参数是有用的. 各位请记下来.        

注意2: 由于libvirt是编译安装的, 所以在系统启动的时候不会自启动. 请手动启动libvirtd进程(root权限).    

2. 编译,安装openvswitch 1.6.1       
3.   
	    $ sudo apt-get install linux-headers-`uname -r`
	    $ ./configure --prefix=/usr --sysconfdir=/etc --localstatedir=/var --libdir=/usr/lib --with-linux=/lib/modules/`unam -r`/build
	    $ make
	    $ sudo make install    
 
注意1: 在linux kernel 3.3或以上, 在执行configure时, 不需要加入 —with-linux 参数.    

3. 启动openvswitch     
在linux kernel 3.3版本以前, 需要先将bridge模块卸载, 再载入刚刚编译的openvswitch模块.     

	    lsmod | grep bridge > /dev/null && sudo rmmod bridge
	    insmod datapath/linux/openvswitch_mod.ko
	    insmod datapath/linux/brcompat_mod.ko
在初次启动openvswitch需要初始化一下数据库:

	    $ sudo mkdir -p /usr/local/etc/openvswitch
	    $ ovsdb-tool create /usr/local/etc/openvswitch/conf.db vswitchd/vswitch.ovsschema    

之后就是启动openvswitch的服务进程
    
	    $ sudo ovsdb-server --remote=punix:/usr/local/var/run/openvswitch/db.sock --remote=db:Open_vSwitch,manager_options --pidfile --detach
	    $ sudo ovs-vsctl --no-wait init
	    $ sudo ovs-vswitchd --pidfile --detach
	    $ sudo ovs-brcompated --pidfile --detach

到此, 服务应该启动完毕. 我们可以用 ovs-vsctl show 来验证一下是否启动成功了.   

4. 创建openvswitch网桥,划分子网    
先划分好网络: 存在PC1, PC2. 在PC1上启动VMa, VMb, 在PC2上启动 VMc, VMd.
VMa, VMc 划分到VLAN 1. VMb, VMd划分到VLAN 2.

在PC1上

    $ sudo brctl addbr br0
    $ sudo brctl addif br0 eth0
    $ sudo ifconfig eth0 0.0.0.0
    $ sudo ifconfig br0 192.168.1.100/24 up
在PC2上

    $ sudo brctl addbr br0
    $ sudo brctl addif br0 eth0
    $ sudo ifconfig eth0 0.0.0.0
    $ sudo ifconfig br0 192.168.1.101/24 up


5. 启动vm到指定网桥     
6. 
6. 将启动VM的xml文件中的网络配置改为使用bridge, 指向br0.       
之后启动虚拟机, 将VMa, VMb, VMc, VMd的ip对应改为192.168.1.2/24, 192.168.1.3/24,     192.168.1.4/24, 192.168.1.5/24.
用简单的ping命令就可以测试VLAN了.    
VMa与VMc之间能够互通.    
VMb与VMd之间能够互通.    
但是VMa与VMb之间不能互通.   
这样就实现了简单的VLAN..   
