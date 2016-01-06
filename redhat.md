
###Software Define Infrastructure  
Scope : 
1. Software ecosystem + solution pererence architectures 
2. SSDs Intel Cache Acceleration Software
3. Processor Platforms : Intel Storage Acceleration Library (ISA-L) & Storage DPDK
4. Intel Quick Assist Technology
5. Networking And Fabric 
6. Server Boards& Systems

###What next?
● vhost-user virtual interface driver
● virtio  performance enhancements
● Ability to use real-time KVM
● Configurable  thread policy:
○ avoid
 - do not place on host that has hyperthreads
○ separate
 - if on host that has hyperthreads, avoid using threads from different cores
○ isolate
 - like separate, but do not allow another guest to use threads on the same CPU core
○ prefer
 - if on host that has hyperthreads, prefer using threads from the same cores (current behaviour)

### Red Hat Gluster Storage performance
Erasure Coding, NFS-Ganesha, RDMA, SSD support, Snapshots

### RHEV Hypervisor 7: Now and the Future


