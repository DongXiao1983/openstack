####Security focused

- Security domains : 
	- 	Public/Private: public network , API calls, 
	- 	Management:  services interact
	- 	Data:  information pertaining to the storage services within OpenStack. The dat   
- Hypervisor-security :  
- Baremetal security : Cloud as a Service  
- Networking Security :  
	- 	Firewalls
	- 	Overlay interconnects for joining separated tenant networks
	- 	Routing through or avoiding specific networks
- Multi-site security   
- OpenStack components :  extra options 

####Compute focused   
	
- High performance computing (HPC)
- Big data analytics using Hadoop or other distributed data stores
- Continuous integration/continuous deployment (CI/CD)
- Signal processing for network function virtualization (NFV)  

####Storage focused 

- Monitoring of physical hardware resources.  
- Monitoring of storage resources such as available storage, memory, and CPU.
- Monitoring of advanced storage performance data to ensure that storage systems are performing as expected.
- Monitoring of network resources for service disruptions which would affect access to storage.
- Centralized log collection.
- Log analytics capabilities.
- Ticketing system (or integration with a ticketing system) to track issues.
- Alerting and notification of responsible teams or automated systems
- which remediate problems with storage as they arise.
- Network Operations Center (NOC) staffed and always available to resolve issues.

####Network focused 
- Network service offerings
- Network management functions
- High speed and high volume transactional systems
- Network misconfigurations 	
- Network tuning  
- Single Point Of Failure (SPOF)  
- Layer-2 architecture limitations   
	- Number of VLANs is limited to 4096.
	- The number of MACs stored in switch tables is limited.  
	- You must accommodate the need to maintain a set of layer-4 devices to handle traffic control.  
	- MLAG, often used for switch redundancy, is a proprietary solution that does not scale beyond two devices and forces vendor lock-in.  
	- It can be difficult to troubleshoot a network without IP addresses and ICMP.  
	- Configuring ARP can be complicated on large layer-2 networks.  
	- All network devices need to be aware of all MACs, even instance MACs,so there is constant churn in MAC tables and network state changes asinstances start and stop.  
	- Migrating MACs (instance migration) to different physical locations are a potential problem if you do not set ARP table timeouts properly.    
- Layer-3 architecture limitations
- Redundant networking: ToR switch high availability risk analysis


####Multi-site

####Hybrid

####Massively scalable

####Specialized cases 
   
- Multi-hypervisor example  
- Specialized networking example  
- Software-defined networking   
- OpenStack on OpenStack  
- Specialized hardware   
