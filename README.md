# Docker Swarm VirtualBox

Create local swarm in virtualbox <b>ubuntu16</b>(ubuntu branch) and/or <b>centos7</b>(master) VM, using vagrant & ansible


#### Step 1:   
create a network adapter under globaltools -> host network manager    
#### Step 2:   
configure adapter manualy and choose an ip address in the samme range as the host, and enable DHCP server  
#### Step 3:   
search and replace xx.xx.xxx with an ip range specified in previous step using  
<b> find . -name 'hosts' -print -exec sed -i.bak 's/xx.xx.xxx/*new ip address range*/g' {} \\; </b>    
#### Step 4:    
<b> ssh-keygen -t rsa -b 4096 -f ./keys/id_rsa  </b>       
*generates ssh keys to be later on of ansible*     
#### Step 5:     
<b> vagrant up </b>  
*brings up VMs*    
#### Step 6:    
<b> ssh -oStrictHostKeyChecking=no vagrant@*ipadd* </b>  
*adds ssh fingerprint to known_hosts, repeat for both manager and host*  
#### Step 7:    
<b> ansible-playbook docker_playbook.yml </b>  
*installs docker-ce*  
#### Step 8:    
<b> ansible-playbook swarm_playbook.yml </b>    
*configures swarm on manager and workers*  

## Login into manager01 and start labborating swarm
*Now you can ssh into manager01 machine and laborate with swarm :)*   
<b> ssh -i keys/id_rsa vagrant@*manager ip address* </b>   

*with in the manager machine run*   
<b> sudo docker node ls </b>  
*this should return both manager and worker nodes*  
     