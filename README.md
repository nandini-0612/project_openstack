# ansible-openstack-project
This project proposes a solution that leverages OpenStack, automation tools, and network design principles to simplify the process. The solution includes the deployment of service and proxy nodes, utilizing load balancing technologies such as HAproxy and Nginx to distribute incoming traffic. High availability is ensured through the implementation of Keepalived-VRRP, reducing the risk of single-point failure. Prometheus and Grafana are integrated for monitoring, providing real-time insights into system metrics.

# prerequisites
- Openstack version: 5.8
- Access to cleura cloud in karlskrona region (is preffered)

# Webservers host:
- python flask application "service.py" running on port 5000.
- SNMP application running on port 6000.
- Node_exporter running on 9100

# Bastion Host:
- prometheus running on port 9090.
- Grafana running on port 3000.

#### ATTENTION ####
We are supposed to generate ssh keypair to run the below scripts using "ssh-keygen -t rsa -b 4096". 
After the creation of public and private key, we keep our public key in the working directory where all the necessary scripts for deployment and cleanup process are present while the private key is to be kept in "~/.ssh" folder.
 It is also adviced to not use ".pub" extension while running the script

# USAGE:

1. Install script:

The command used to run the install script and what the output looks like is shown below:

shravya@LAPTOP-3P7P8VTT:~/ansible-openstack-project$ ./install nso--rc nso id_rsa <br />
[Sun May 28 09:57:13 CEST 2023] Starting process of installation for tag: nso using credentails from nso--rc <br />
[Sun May 28 09:57:15 CEST 2023] created keypair nso_key <br />
[Sun May 28 09:57:17 CEST 2023] checking for floating_ips <br />
[Sun May 28 09:57:17 CEST 2023] no floating IP available. Creating new floating IPs .. <br />
[Sun May 28 09:57:25 CEST 2023] created 2 new floating IPs <br />
[Sun May 28 09:57:30 CEST 2023] created network with name nso_network <br />
[Sun May 28 09:57:37 CEST 2023] Creating new router with tag name nso_router... <br />
[Sun May 28 09:57:45 CEST 2023] Creating new subnet pool with tag name nso_subnet_pool ... <br />
[Sun May 28 09:57:55 CEST 2023] Creating new subnet with tag name nso_subnet... <br />
[Sun May 28 09:58:02 CEST 2023] Connecting subnet-to-router <br />
[Sun May 28 09:58:15 CEST 2023] Connecting router to ext gateway <br />
[Sun May 28 09:58:23 CEST 2023] Creating master and backup ports... <br />
[Sun May 28 09:58:57 CEST 2023] Creating Security groups... <br />
[Sun May 28 09:59:11 CEST 2023] Did not detect nso_bastion. Creating nso_bastion... <br />
[Sun May 28 09:59:48 CEST 2023] Finished creating nso_bastion... <br />
[Sun May 28 09:59:53 CEST 2023] Did not detect nso_haproxy_1. Creating nso_haproxy_1... <br />
[Sun May 28 10:00:16 CEST 2023] Finished creating nso_haproxy_1... <br />
[Sun May 28 10:00:18 CEST 2023] Did not detect nso_haproxy_2. Creating nso_haproxy_2... <br />
[Sun May 28 10:00:42 CEST 2023] Finished creating nso_haproxy_2... <br />
[Sun May 28 10:01:10 CEST 2023] creating vrrp port <br />
[Sun May 28 10:01:27 CEST 2023] enabling ports <br />
[Sun May 28 10:01:56 CEST 2023] created dev_server nso_dev1 <br />
[Sun May 28 10:02:18 CEST 2023] created dev_server nso_dev2 <br />
[Sun May 28 10:02:43 CEST 2023] created dev_server nso_dev3 <br />
[Sun May 28 10:02:43 CEST 2023] building ssh_config file <br />
[Sun May 28 10:02:43 CEST 2023] running playbook <br />

2. Operate script:

The command used to run the operate script and what the output looks like is shown below:

shravya@LAPTOP-3P7P8VTT:~/ansible-openstack-project$ ./operate nso--rc nso id_rsa <br />
[Sun May 28 10:17:38 CEST 2023] Starting operation process to handle dev servers for tag:nso using nso--rc for credentails. <br />
[Sun May 28 10:17:43 CEST 2023] Currently running dev servers count is 3 which is same as required 3 <br />
[Sun May 28 10:17:49 CEST 2023] Currently running dev servers count is 3 which is same as required 3 <br />
[Sun May 28 10:17:54 CEST 2023] Currently running dev servers count is 3 which is same as required 3 <br />
[Sun May 28 10:18:24 CEST 2023] Currently running dev servers count is 3 which is less then required number 5 created server nso_dev4 <br />
[Sun May 28 10:18:53 CEST 2023] Currently running dev servers count is 3 which is less then required number 5 created server nso_dev5 <br />
[Sun May 28 10:18:53 CEST 2023] starting creation of new config file <br />
[Sun May 28 10:19:00 CEST 2023] nso_haproxy_1 exists adding in new config file... <br />
[Sun May 28 10:19:04 CEST 2023] nso_haproxy_2 exists adding in new config file... <br />
[Sun May 28 10:19:13 CEST 2023] adding nso_dev5 into the new config file <br />
[Sun May 28 10:19:16 CEST 2023] adding nso_dev4 into the new config file <br />
[Sun May 28 10:19:19 CEST 2023] adding nso_dev3 into the new config file <br />
[Sun May 28 10:19:22 CEST 2023] adding nso_dev2 into the new config file <br />
[Sun May 28 10:19:25 CEST 2023] adding nso_dev1 into the new config file <br />
[Sun May 28 10:19:25 CEST 2023] running the playbook... <br />
[Sun May 28 10:20:29 CEST 2023] Currently running dev servers count is 5 which is same as required 5 <br />
[Sun May 28 10:20:39 CEST 2023] Currently running dev servers count is 5 which is same as required 5 <br />
[Sun May 28 10:20:58 CEST 2023] Running dev servers count is 5 which is greater then required 2 deleted server nso_dev5 <br />
[Sun May 28 10:21:11 CEST 2023] Running dev servers count is 5 which is greater then required 2 deleted server nso_dev4 <br />
[Sun May 28 10:21:33 CEST 2023] Running dev servers count is 5 which is greater then required 2 deleted server nso_dev3 <br />
[Sun May 28 10:21:33 CEST 2023] starting creation of new config file <br />
[Sun May 28 10:21:41 CEST 2023] nso_haproxy_1 exists adding in new config file... <br />
[Sun May 28 10:21:45 CEST 2023] nso_haproxy_2 exists adding in new config file... <br />
[Sun May 28 10:21:52 CEST 2023] adding nso_dev2 into the new config file <br />
[Sun May 28 10:21:55 CEST 2023] adding nso_dev1 into the new config file <br />
[Sun May 28 10:21:55 CEST 2023] running the playbook... <br />

3. Cleanup script:

The command used to run the cleanup script and what the output looks like is shown below:

shravya@LAPTOP-3P7P8VTT:~/ansible-openstack-project$ ./cleanup nso--rc nso id_rsa <br />
[Sun May 28 10:25:13 CEST 2023] Starting process to cleaning up for tag: nso using credentails from nso--rc <br />
[Sun May 28 10:25:26 CEST 2023] Deleted server nso_dev2 <br />
[Sun May 28 10:25:41 CEST 2023] Deleted server nso_dev1 <br />
[Sun May 28 10:25:52 CEST 2023] Deleted server nso_haproxy_2 <br />
[Sun May 28 10:26:07 CEST 2023] Deleted server nso_bastion <br />
[Sun May 28 10:26:20 CEST 2023] deleted floating ips <br />
[Sun May 28 10:26:46 CEST 2023] deleted ports <br />
[Sun May 28 10:27:09 CEST 2023] deleted router <br />
[Sun May 28 10:27:19 CEST 2023] deleted subnet and subnetpool <br />
[Sun May 28 10:27:22 CEST 2023] delete security group <br />
[Sun May 28 10:27:24 CEST 2023] deleted network <br />
[Sun May 28 10:27:27 CEST 2023] deleted keypair <br />
[Sun May 28 10:27:27 CEST 2023] ########END####### <br />

4. testing: 

testing of CURL: <br />
shravya@LAPTOP-3P7P8VTT:/ansible-openstack-project$ curl haproxy_floating_ip:5000 <br />
08:23:07 10.0.0.5:53724 -- 10.0.0.11 (nso-dev1) 88 <br />
shravya@LAPTOP-3P7P8VTT:/ansible-openstack-project$ curl haproxy_floating_ip:5000 <br />
08:23:12 10.0.0.5:44650 -- 10.0.0.12 (nso-dev2) 66 <br />

testing of SNMP: <br />
shravya@LAPTOP-3P7P8VTT:~/ansible-openstack-project$ snmpwalk -v 2c -c <haproxy_floating_ip>:6000 <br />
iso.3.6.1.2.1.1.1.0 = STRING: "Linux nso-dev1 5.4.0-26-generic #30-Ubuntu SMP Mon Apr 20 16:58:30 UTC 2020 x86_64" <br />
iso.3.6.1.2.1.1.2.0 = OID: iso.3.6.1.4.1.8072.3.2.10 <br />
iso.3.6.1.2.1.1.3.0 = Timeticks: (90494) 0:15:04.94 <br />
iso.3.6.1.2.1.1.4.0 = STRING: "Me <me@example.org>" <br />
iso.3.6.1.2.1.1.5.0 = STRING: "nso-dev1" <br />
iso.3.6.1.2.1.1.6.0 = STRING: "Sitting on the Dock of the Bay" <br />
iso.3.6.1.2.1.1.7.0 = INTEGER: 72 <br />
iso.3.6.1.2.1.1.8.0 = Timeticks: (4) 0:00:00.04 <br />
iso.3.6.1.2.1.1.9.1.2.1 = OID: iso.3.6.1.6.3.10.3.1.1 <br />
iso.3.6.1.2.1.1.9.1.2.2 = OID: iso.3.6.1.6.3.11.3.1.1 <br />
iso.3.6.1.2.1.1.9.1.2.3 = OID: iso.3.6.1.6.3.15.2.1.1 <br />
iso.3.6.1.2.1.1.9.1.2.4 = OID: iso.3.6.1.6.3.1 <br />
iso.3.6.1.2.1.1.9.1.2.5 = OID: iso.3.6.1.6.3.16.2.2.1 <br />
iso.3.6.1.2.1.1.9.1.2.6 = OID: iso.3.6.1.2.1.49 <br />
iso.3.6.1.2.1.1.9.1.2.7 = OID: iso.3.6.1.2.1.4 <br />
iso.3.6.1.2.1.1.9.1.2.8 = OID: iso.3.6.1.2.1.50 <br />
iso.3.6.1.2.1.1.9.1.2.9 = OID: iso.3.6.1.6.3.13.3.1.3 <br />
iso.3.6.1.2.1.1.9.1.2.10 = OID: iso.3.6.1.2.1.92 <br />
iso.3.6.1.2.1.1.9.1.3.1 = STRING: "The SNMP Management Architecture MIB." <br />
iso.3.6.1.2.1.1.9.1.3.2 = STRING: "The MIB for Message Processing and Dispatching." <br />
iso.3.6.1.2.1.1.9.1.3.3 = STRING: "The management information definitions for the SNMP User-based Security Model." <br />
iso.3.6.1.2.1.1.9.1.3.4 = STRING: "The MIB module for SNMPv2 entities" <br />
iso.3.6.1.2.1.1.9.1.3.5 = STRING: "View-based Access Control Model for SNMP." <br />
iso.3.6.1.2.1.1.9.1.3.6 = STRING: "The MIB module for managing TCP implementations" <br />
iso.3.6.1.2.1.1.9.1.3.7 = STRING: "The MIB module for managing IP and ICMP implementations" <br />
iso.3.6.1.2.1.1.9.1.3.8 = STRING: "The MIB module for managing UDP implementations" <br />
iso.3.6.1.2.1.1.9.1.3.9 = STRING: "The MIB modules for managing SNMP Notification, plus filtering." <br />
iso.3.6.1.2.1.1.9.1.3.10 = STRING: "The MIB module for logging SNMP Notifications." <br />
iso.3.6.1.2.1.1.9.1.4.1 = Timeticks: (4) 0:00:00.04 <br />
iso.3.6.1.2.1.1.9.1.4.2 = Timeticks: (4) 0:00:00.04 <br />
iso.3.6.1.2.1.1.9.1.4.3 = Timeticks: (4) 0:00:00.04 <br />
iso.3.6.1.2.1.1.9.1.4.4 = Timeticks: (4) 0:00:00.04 <br />
iso.3.6.1.2.1.1.9.1.4.5 = Timeticks: (4) 0:00:00.04 <br />
iso.3.6.1.2.1.1.9.1.4.6 = Timeticks: (4) 0:00:00.04 <br />
iso.3.6.1.2.1.1.9.1.4.7 = Timeticks: (4) 0:00:00.04 <br />
iso.3.6.1.2.1.1.9.1.4.8 = Timeticks: (4) 0:00:00.04 <br />
iso.3.6.1.2.1.1.9.1.4.9 = Timeticks: (4) 0:00:00.04 <br />
iso.3.6.1.2.1.1.9.1.4.10 = Timeticks: (4) 0:00:00.04 <br />
iso.3.6.1.2.1.25.1.1.0 = Timeticks: (131586) 0:21:55.86 <br />
iso.3.6.1.2.1.25.1.2.0 = Hex-STRING: 07 E7 05 1C 08 17 31 00 2B 00 00 <br />
iso.3.6.1.2.1.25.1.3.0 = INTEGER: 393216 <br />
iso.3.6.1.2.1.25.1.4.0 = STRING: "BOOT_IMAGE=/boot/vmlinuz-5.4.0-26-generic root=PARTUUID=aaccf730-1ff9-4b7e-b952-d15c757623bf ro console=tty1 console=ttyS0
" <br />
iso.3.6.1.2.1.25.1.5.0 = Gauge32: 0 <br />
iso.3.6.1.2.1.25.1.6.0 = Gauge32: 91 <br />
iso.3.6.1.2.1.25.1.7.0 = INTEGER: 0 <br />
iso.3.6.1.2.1.25.1.7.0 = No more variables left in this MIB View (It is past the end of the MIB tree) <br />



