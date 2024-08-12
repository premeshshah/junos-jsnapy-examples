# junos-jsnapy-examples
Juniper JSNAPy Pre/Post checks Automation Examples


Achievements
Pre/Post checks 
	JSNAPy 
		SERVICE PROVIDER based current checks.
		JSNAPy based checks
		JSNAPy detailed command/output mapping
References
Docker image 
	Customized image 
	How to Install / Load Docker image
	Execute  
		Map local drive to Docker image
		Login to Docker 
		Execute playbook



Achievements
1.	Ansible Tower based playbook.
2.	Two playbooks (1 for PRE and other for POST and CHECK)
3.	Twenty commands check including Interfaces, OSPF, BGP and MPLS/RSVP and System with around 39 checks.
4.	Additional config and inventory collection for PRE and POST
5.	Detailed JSNAPy output in JSON format for future reference.


**Pre/Post checks**


**JSNAPy**

•	Automated network state verification using JSNAPy. The JSNAPy python module was used to capture the operational state of a network and compare it to a working 'golden' state based on user-defined test cases if a change in the network causes part of the network to fail.  
•	Using the same command-line structure as before, a separate playbook was created with the same user-defined parameters such as the project and phase name. A set of test files were used and defined in a test configuration file to allow JSNAPy tests to be run.  
•	The JSNAPy "snap-pre" functionality was used capture the golden operational state and save the snapshotted .xml files in the correct project directory by creating a separate jsnapy.cfg file in each project directory.  
•	The JSNAPy "check" functionality was used to compare running operational states to the golden state and display modifications that may have caused a failure in the network.  

![image](https://github.com/user-attachments/assets/8159b9ec-3b5c-462c-8a99-3c136e873961)



| SERVICE PROVIDER based current checks |
| --- |
| show pfe terse |
| --- |
| show chassis fpc |
| --- |
| show chassis fpc pic-status |
| --- |
| show interface terse | no-more |
| --- |
| show ospf nei | resolve | no-more |
| --- |
| show rsvp nei | resolve | no-more |
| --- |
| show bgp summ | resolve | no-more |
| --- |
| show route summary | no-more |
| --- |
| show route | count |
| --- |
| show ldp neighbor | resolve | no-more |
| --- |
| show interface description | no-more |
| --- |
| show mpls lsp ingress | resolve | no-more |
| --- |
| show l2circuit connection | resolve | no-more |
| --- |




![image](https://github.com/user-attachments/assets/c0957c02-261a-4c22-a757-25c810e95e87)


   
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



