ipmitool -H 10.70.48.7 -U admin -P admin -I lan sensor list   
https://blueprints.launchpad.net/ceilometer/+spec/ceilometer-cloudwatch-api   
https://www.mirantis.com/blog/openstack-monitoring/   
https://wiki.openstack.org/wiki/ResourceMonitorAlertsandNotifications   


Fault and Performance Management OEM tools

In order to provide Fault and Performance management functionality in NCIO infrastructure, NCIO team performed several studies using multiple OEM components. Zabbix is chosen as the tool for monitoring purposes. It can simultaneously monitor hardware and software. Along with storing the data, visualization features are available (overviews, maps, graphs, screens, etc), as well as very flexible ways of analyzing the data for the purpose of alerting.  Zabbix features shortly presented bellow:    
Monitoring of network services (SMTP, POP3, HTTP, NNTP, PING, etc.)     
Monitoring of host resources (processor load, disk usage, etc.)     
User can write their own plugin to monitor custom parameters    
Parallelized service checks     
Contact notifications when service or host problems occur and get resolved (via email, SMS,windows pop-up or user-defined method)     
Ability to define action on triggers to be run during service or host events for proactive problem resolution     
Automatic log file rotation    
Support for implementing redundant monitoring hosts     
Log file monitoring     
Hypervisor monitoring (VMware)    
Rich web interface for viewing current network status, notification and problem history, log file, etc.    

Northbound interface towards NetAct is done using  snmp, no additional OEM component is used for NBI.   
