# Automation-in-Linux-using-Ansible
Configuration management in Linux using Ansible

Ansible

Sudo apt update && apt install ansible

<img width="712" alt="Screenshot 2023-06-21 at 22 18 24" src="https://github.com/Mamiololo01/Automation-in-Linux-using-Ansible/assets/67044030/a277d7ef-42b9-42d5-a3a3-23797dea80bd">


ansible —version

<img width="833" alt="Screenshot 2023-06-21 at 22 19 27" src="https://github.com/Mamiololo01/Automation-in-Linux-using-Ansible/assets/67044030/129ae59c-244a-4e51-bae1-c9ff35fd579f">

Prerequisites

One controller machine

Two or more VM 

SSH between CM and VM(managed host)

Ssh -l username -p 22 [ipadd_remote_vm]

Ansible compote

Inventory

Modules

Tasks

Playbooks

vi hosts

{severs}
vps1 ansible_host=ipadd

vps2 ansible_host=ipadd

Ansible -I ./hosts --list-hosts all

Sudo apt install sshpass

Edit the conf file of ansible

sudo vim /etc/ansible/ansible.cfg

Uncomment host_key_checking = False

ansible -I ./hosts --list-hosts vps1 -m ping -u username -k

You will get a ping pong green message that confirm ssh to the managed host

To test all severs

ansible -I ./hosts --list-hosts servers -m ping -u username -k

Useful information about the remote host

ansible -I ./hosts --list-hosts servers -m setup -u username -k

Edit the inventory file

vi hosts

{severs}
vps1 ansible_host=ipadd

vps2 ansible_host=ipadd

{server:vars}

ansible_user=username

ansible_ssh_pass=ansible123-

ansible_port=22

Save and exit

Run the command again

ansible -I ./hosts --list-hosts servers -m setup 

If each vm has diff username and password

{severs}

vps1 ansible_host=ipadd ansible_user=student

vps2 ansible_host=ipadd

Ansible ad-hoc commands

ansibke -I ./hosts servers -m shell -a “df-h” -u student -k

ansible -I ./hosts servers -m shell -a “df-h” -f

Number of memory on each server

ansible -I ./hosts servers -m shell -a “lumem” -f

ansible -I ./hosts servers -m shell -a “free -m” 

ansible -I ./hosts servers -m shell -a “ip add show dev eth0” 

SCRIPT MODULE

A new backup script. 

Vim backup.sh

 #! /bin/bash

Tar -czf /root/etc-$(date +%F).tar.gz /etc

Save

ansible -I ./hosts servers -m script-a “./backup.sh” —become -K

login to VM1 to check if archive was created

ssh -l username -p 22 [ipadd_remote_vm

sudo ls /root/

APT MODULE

Automate software installation

Install Nmap on the managed host

ansible -I hosts servers -m apt -a “name=nmap state=present update_cache=true” -u student -k —become -K

nmap will be installed on all the VM

Use state=absent  and purge=yes to remove nmap on the remote VM

ansible -I ./hosts servers -m apt -a “name=nmap state=absent purge=yes update_cache=true” —become

Full upgrade on all systems

ansible -i ./hosts servers -m apt -a “upgrade=full” —become

ansible -i ./hosts servers -m apt -a “name=nginx state=present update_cache=true” —become

state=stop, state=restart to stop and restart the nginx on all remote VMs at the same time. enabled=yes/no means to start on boot or not.


USER MODULE

ansible -I ./hosts servers -m group -a “name=developers state=present” —become

tail -n 3 /etc/group

ansible -I ./hosts servers -m group -a “name=developers state=absent” —become

tail -n 3 /etc/group

Add user to the group

ansible -I ./hosts servers -m user -a “name=John state=present groups=developer create home=yes comment=\“new user\”” —become

tail -n 3 /etc/passwd

To generate ssh key

ansible -I ./hosts servers -m user -a “name=John state=present groups=developer create home=yes comment=\“new user\” shell=/bin/bash generate_ssh_key=yes  ssh_key_bits=2048”  —become


Task Scheduling using Cron

editing the current user’s crontab file

crontab -e
		 
listing the current user’s crontab file

crontab -l

removing the current user’s crontab file

crontab -r		 

COMMON EXAMPLES
run every minute

* * * * * /path_to_task_to_run.sh
		 
run every hour at minute 15

15 * * * * /path_to_task_to_run.sh
		 
run every day at 6:30 PM

30 18 * * * /path_to_task_to_run.sh
		 
run every Monday at 10:03 PM

3 22 * * 1 /path_to_task_to_run.sh
	 
run on the 1st of every Month at 6:10 AM

10 6 1 * * /path_to_task_to_run.sh
		 
run every hour at minute 1, 20 and 35

1,20,35 * * * * /path_to_task_to_run.sh
		 
run every two hour at minute 10

10 */2 * * * /path_to_task_to_run.sh
		 
run once a year on the 1st of January at midnight

@yearly /path_to_task_to_run.sh
		 
run once a month at midnight on the first day of the month

@monthly /path_to_task_to_run.sh
		 
run once a week at midnight on Sunday

@weekly /path_to_task_to_run.sh

once an hour at the beginning of the hour

@hourly /path_to_task_to_run.sh
		 
run at boot time

@reboot /path_to_task_to_run.sh
	 
All scripts in following directories will run as root at that interval:

/etc/cron.hourly

/etc/cron.daily

/etc/cron.hourly

/etc/cron.monthly
/etc/cron.weekly
