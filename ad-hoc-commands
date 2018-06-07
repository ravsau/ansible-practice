# ansible-practice

Step 1: Install ansible
sudo yum install ansible

Step 2: Add a user ansible on the control machine
sudo useradd ansible

Step 3: Add a password for the user ansible
sudo passwd ansible

Step 4: Add the info for remote nodes and localhost on the /etc/ansible/hosts file

[webservers]
172.31.15.67 user=ansible
172.31.4.90  user=ansible
localhost    user=ansible

Step 5: Open /etc/ansible/ansible.cfg and add this line:
host_key_checking = False





#Ping all the remote nodes
ansible all -m ping --ask-pass

# Install apache web server 
#become means that the ansible user will become root to perform installation
ansible all -m yum -a "name=httpd state=present" --ask-pass --become

# Start the apache web server
ansible all -m service -a "name=httpd state=started" --ask-pass --become
