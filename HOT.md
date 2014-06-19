### Heat Orchestration Template (HOT) Guide  
[Guide] (http://docs.openstack.org/developer/heat/template_guide/hot_guide.html)  
[Specification](http://docs.openstack.org/developer/heat/template_guide/hot_spec.html#hot-spec)    

[Wik](http://en.wikipedia.org/wiki/Orchestration_(computing))上给出的定义：
Cloud service orchestration therefore is the:

> 1. Composing of architecture, tools and processes by humans to deliver a defined service    
> 2. Stitching of software and hardware components together to deliver a defined Service    
> 3. Connecting and automating of work flows when applicable to deliver a defined service    

It is critical in the delivery of cloud services because:

> 1. Cloud is all about scale – automated work flows are essential[weasel words]    
> 2. Cloud service delivery includes fulfillment assurance and billing    
> 3. Cloud services delivery entails work flows in various technical and business domains 


Orchestration就是将服务中涉及的工具、流程、结构描述成一个可执行的流程，结合软硬件的接口，使之可以按照描述自动化、可重复地交付。具体到Cloud Orchestration，主要就是通过定义一系列的元数据来描述交付过程中涉及的环境管理(OS. network, storage ..)、软件包管理、配置管理以及监控和升级操作，以便自动化和标准化地完成从申请资源到交付/升级服务的整个生命周期。    



1. packet CRC problem
2. Event machine proposal -> satish
    -- internal -> SDN
3. DPDK Solution -> all
4. Scalable Dat Plane
   -- Asymmetric, packet 
5. PMD frame work, PMD-PCAP, IGb_UIO,UIO
6. OSV_DPDK
7. DPDK dependancy with NICS
8. Control plane -> compotetion intensive
9. Data plane -> lertency
10. BSC + FCC  Transpot -> reuse
11.internal study: MBB, CORE, SRAN
12.



Please check these videos.

AT&T & Ericsson  /// OpenStack videos: 

http://www.youtube.com/watch?v=eeaTQsZdxHE&sns=e

https://www.openstack.org/summit/openstack-summit-atlanta-2014/session-videos/presentation/openstack-as-the-key-engine-of-nfv

Ericsson

