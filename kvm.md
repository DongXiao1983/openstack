#[KVM核心基础功能]

硬件平台和软件版本说明

1.硬件平台：硬件要支持辅助虚拟化（Intel VT-x）

2.KVM内核:在linux.git源码仓库中执行

	# git tag	
	v3.4-rc6	
	v3.4-rc7	
	v3.5-rc1
	#git checkout v3.5

在kvm.git中，没有v标签，可以

	# git log
	# git checkout 73bcc499

在qemu-kvm.git中，可以使用git tag

在编译qemu时，configure加上 –disable-sdl

使用qemu命令行，默认开启对KVM支持，# qemu-kvm 查看KVM默认打开，需要启动QEMU命令行上加上-enable-kvm

##CPU配置

QEMU/KVM为客户机提供一套完整的硬件系统环境

客户机所拥有的CPU是vCPU

在KVM环境中，每个客户机都是一个标准的Linux进程(QEMU进程)

每一个vCPU在宿主机中是QEMU进程派生的一个普通线程

客户机的命令模拟实现：

> 1.用户模式
> 
> 处理I/O模拟和管理，由QEMU代码实现
> 
> 2.内核模式
> 
> 处理需要高性能和安全相关指令。处理客户模式下I/O指令或其他特权指令引起的退出，处理影子内存管理
> 
> 3.客户模式
> 
> 执行Guest中的大部分指令，I/O和一些特权指令除外（引起VM-Exit，被hypervisor截获并模拟）

## SMP支持

多处理，多进程

逻辑CPU个数

	# cat /proc/cpuinfo | grep “pro”|wc -l

多线程支持

	# cat /proc/cpuinfo | grep -qi “core id” |echo $?

实际CPU个数

	# cat /proc/cpuinfo | grep “physical id” |sort | uniq | wc -l

每个物理CPU中逻辑CPU的个数

	logical_cpu_per_phy_cpu=$(cat /proc/cpuinfo |grep “siblings”| sort | uniq | awk -F: ‘{print $2}’)

	echo $logical_cpu_per_phy_cpu

在qemu的命令行中，模拟客户机的SMP系统，参数

	-qemu n[,maxcpus=cpus][,cores=cores][,threads=threads][,sockets=sockets]

n 逻辑cpu数量

maxcpus 最大可被使用的cpu数量

cores 每个cpu socket上的core数量

threads 每个cpu core上的线程数

sockets 客户机中看的总的cops socket数量



CPU模型

查看当前qemu支持的所有CPU模型

	# qemu-system-x86_64 -cpu ?

启动镜像时，可以使用上命令的结果的CPU模型启动

	# qemu-system-x86_64 /root/…/…img -cpu SandyBridge

隔离其他CPU不供客户机使用

grub 文件

	root=UUID…. isolcpus=2,3

查看隔离成功

	# ps -eLo psr|grep 0|wc-l

然后改变0，检查各个CPU的进程数

把一个有两个的vCPU的客户机绑定到宿主机的两个CPU上

	# qemu-system-x86_64 rhel6u3.img -smp 2 -m 512 -daemonize

查看代表vCPU的QEMU的线程

	#ps -eLo ruser,pid,ppid,lwp,psr,args |grep qemgrep -v grep

绑定整个客户机的QEMU进程，运行在CPU2上

	#taskset -p 0×4 3963

	#绑定第一个cCPU线程，使其运行在cpu2上

	# taskset -p 0×4 3967

第二个

	#taskset -p 0×8 3968

##内存配置

QEMU启动命令


	# free -m 
	# dmesg|grep Memory

内存转换

> 客户机->宿主机(GVA->GPA)执行者：客户机操作系统
> 
> 客户机<-宿主机(GVA<-GPA)执行者：hypervisor

在EPT加入之后，直接在硬件上支持虚拟化内存地址转换

查看支持EPT VPID

	# grep ept /proc/cpuinfo
	# cat /sys/module/kvm_intel/parameters/ept
	# cat /sys/module/kvm_intel/parameters/vpid

加载ept vpid模块

	# modprobe kvm_intel ept=0,vpid=0
	# rmmod kvm_intel
	# modprobe kvm_intelept=1,vpid=1

大页

x86默认提供4kb内存页面

x86_64支持2MB大小的大页

	# getconf PAGESIZE
	# cat /proc/meminfo

挂载大页

	# mount -t hugetlbfs hugetlbfs /dev/hugepages
	# sysctl vm.nr_hugepages=1024

然后让qemu启动项内存参数使用大页

	-mem-path /dev/hugepages

挂载大页内存可以显著提高客户机性能，但是，不能使用swap

#### 内存过载使用

> 1.内存交换(swaping)    
> 2.气球(ballooning):通过virio_balloon驱动来实现宿主机Hyperviosr和宿主机的合作    
> 3.页共享(page sharing):通过KSMkernel Samepage Merging合并多个客户机使用的相同内存页。    


