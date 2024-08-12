# junos-jsnapy-examples
Juniper JSNAPy Pre/Post checks Automation Examples


•	Achievements
•	Pre/Post checks 
	o	JSNAPy 
			SERVICE PROVIDER based current checks.
			JSNAPy based checks
			JSNAPy detailed command/output mapping
•	References
•	Docker image 
	o	Customized image 
	o	How to Install / Load Docker image
	o	Execute  
			Map local drive to Docker image
			Login to Docker 
			Execute playbook



Achievements
1.	Ansible Tower based playbook.
2.	Two playbooks (1 for PRE and other for POST and CHECK)
3.	Twenty commands check including Interfaces, OSPF, BGP and MPLS/RSVP and System with around 39 checks.
4.	Additional config and inventory collection for PRE and POST
5.	Detailed JSNAPy output in JSON format for future reference.
Pre/Post checks
JSNAPy

•	Automated network state verification using JSNAPy. The JSNAPy python module was used to capture the operational state of a network and compare it to a working 'golden' state based on user-defined test cases if a change in the network causes part of the network to fail.  
•	Using the same command-line structure as before, a separate playbook was created with the same user-defined parameters such as the project and phase name. A set of test files were used and defined in a test configuration file to allow JSNAPy tests to be run.  
•	The JSNAPy "snap-pre" functionality was used capture the golden operational state and save the snapshotted .xml files in the correct project directory by creating a separate jsnapy.cfg file in each project directory.  
•	The JSNAPy "check" functionality was used to compare running operational states to the golden state and display modifications that may have caused a failure in the network.  

parameter	type	required	default	choices	comments
action	str	yes	none	•	check
•	snapcheck
•	snap_pre
•	snap_post	The JSNAPy action to perform.
config_file	path	no	none		The filename of a JSNAPy configuration file (in YAML format). The test_files option and the config_file option are mutually exclusive. Either the test_files option or the config_file option is required.
dir	path	no	/etc/jsnapy/testfiles		The path to the directory containing the JSNAPy test file(s) specified by the test_files option or the JSNAPy configuration file specified by the config_file option.

aliases: directory
test_files	list of path	no	none		The filename of file(s) in the dir directory. Each file contains JSNAPy test case definitions. The test_files option and the config_file option are mutually exclusive. Either the test_files option or the config_file option is required.
SERVICE PROVIDER based current checks.
SERVICE PROVIDER based current checks
show pfe terse 
!
show chassis fpc
!
show chassis fpc pic-status
!
show interface terse | no-more
!
show ospf nei | resolve | no-more
!
show rsvp nei | resolve | no-more
!
show bgp summ | resolve | no-more
!
show route summary | no-more
!
show route | count
!
show ldp neighbor | resolve | no-more
!
show interface description | no-more
!
show mpls lsp ingress | resolve | no-more
!
show l2circuit connection | resolve | no-more
!
JSNAPy based checks
JSNAPy based checks
show system core-dumps.
show bgp summary (Group count / Neighbor count / Active-prefix-count delta 5% / bgp-rib-state / peer-state / peer-as)
show lldp neighbors (local-interface and parent interface)
show lacp interfaces (activity status)
show rsvp interface (interface count and status)
show ldp interface (neighbor count / interface and address mapping)
show chassis hardware (serial-number change)
show chassis fpc pic-status (fpc and pic status)
show chassis alarms (list)
show system alarms (list)
show route summary (active-route-count 2% delta / hidden-route-count / protocol change)
show bfd session (state change)
show interfaces (physical oper state / logical address family and IP address)
show chassis environment (status)
show chassis routing-engine (mastership / installed memory size)
show ospf neighbor (list / neighbor state)
show ospf interface (neighbor count / ospf area)
show ospf database (lsa-id and type / advertising-router)
show mpls lsp (lsp-state and list)
show l2circuit connection (list and connection-status)


JSNAPy detailed command/output mapping
Category	CLI	Check	Output
Hardware status	show chassis hardware models	Check serial numbers of all hardware.	Notify previous and new serial numbers for inventory purpose.
	show chassis fpc pic-status	Check status of all hardware (FPC and PIC).	Notify of FPCs or PICs that are not online.
	show chassis environment	Check status of all hardware environments.	Notify if any hardware status has changed.
	show chassis routing- engine	Check mastership status of both REs.	Notify if any RE slot mastership status has changed.
System Status	show chassis alarms	Check if any new alarms are active.	Notify if any new alarms are active.
	show system core-dumps routing-engine both	Check if there are any core files on any of the REs.	Notify if there are any new core files.
Interfaces Status	show interface terse	Check if any oper-status has changed.	Notify if interface, oper-status, or admin-status has changed.		
	show lacp interface	Check if LAG status changed	Notify if LAG interface has changed.			
Route Summary	show route summary	Check if any route table has discrepancy in number of routes.	Notify if number of routes has changed by 20%.			
	show route forwarding- table summary	Check if any forwarding table has discrepancy in number of routes.	Notify if number of routes in forwarding table has changed by 20%			
BFD	show bfd session detail	Check if any BFD session is down.	Notify if BFD session is down.			
OSPF	show ospf interface	Check if any OSPF interface has 0 neighbor (interface which contains “-”).	Notify if any OSPF interface has 0 neighbor.			
	show ospf neighbor	Check if any OSPF neighbor is missing.	Notify if any OSPF neighbor is missing.			
	show ospf database detail	Check if any OSPF router link count is in predefined range.	Notify if any OSPF router link count is out of range.			
BGP	show bgp summary	Check if any BGP neighbor is missing. Plus, if all established plus active prefix delta is more than 20%.	Notify if any BGP neighbor is missing. Or it is not established, or delta is more than 20%.			
L2 Circuits	show l2circuit connections	Check if any l2circuit is not in UP state, also that no l2circuit is missing.	Notify if any l2circuit session is not up.			
LDP	show ldp interface	Check if any LDP interface has 0 neighbor.	Notify if any LDP interface has 0 neighbor.			
	show ldp neighbor	Check if any LDP neighbor is missing.	Notify if any LDP neighbor is missing.			
RSVP	show rsvp interface	Check if any RSVP interface is missing. Plus, if any interface is not in UP state.	Notify if any RSVP interface is missing.			
	show rsvp session	Check if any RSVP session is missing.	Notify if any RSVP session is missing.			
MPLS	show mpls lsp detail	Check if any MPLS LSP is not in UP state, also that no LSP is missing.	Notify if any MPLS LSP session is not up.			
			
			
References
https://hub.docker.com/r/juniper/pyez-ansible/
https://galaxy.ansible.com/juniper/junos
https://stackoverflow.com/questions/23935141/how-to-copy-docker-images-from-one-host-to-another-without-using-a-repository
Ansible for Junos OS Documentation | Juniper Networks
Using Junos Snapshot Administrator in Python (JSNAPy) in Ansible Playbooks | Ansible for Junos OS Developer Guide | Juniper Networks TechLibrary
https://www.juniper.net/documentation/en_US/day-one-books/DO_JSNAPy.zip

Docker image
Customized image 
1.	Image was downloaded from the docker with the image name pyez-ansible.
2.	Juniper Ansible package called juniper.junos is added to the docker image.
3.	JSNAPy location is changed to use the shared directory.

How to Install / Load Docker image
1.	Unzip the file 
2.	Use docker load -i file name 
Execute 
Map local drive to Docker image
Below is the command needed every time docker is not running.
sudo docker run --rm --name jsnapy -it -v ~/.ssh:/root/.ssh -v $PWD:/project -w /project juniper/pyez-ansible ash
Login to Docker 
###docker ps
CONTAINER ID         IMAGE                                               COMMAND                      CREATED      STATUS       NAMES
b4149553ac53         juniper/pyez-ansible:SP      "/usr/bin/entrypoint…"     3 days ago        Up         ansible_jsnapy
###docker exec -it b4149553ac53 ash

Execute playbook
<<<<PRE CHECKS>>>>
ansible-playbook pre_check.yml
<<<<<<NETWORK CHANGE>>>>>>
<<<<POST CHECKS>>>>
ansible-playbook post_check.yml



