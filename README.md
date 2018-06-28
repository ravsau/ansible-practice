# ansible-practice


# What's in this repo?

- Commands to install ansible on Red Hat Enterprise Linux (RHEL)
- A files with ad-hoc ansible commands to perform simple tasks like ping
- 2 playbooks that have 1 tasks each. 1 uses the copy module and 1 uses the shell module
- 1 playbook that calls a role that's contained in copy-role folder
- 1 html file that's used by the copy module

**I'm doing this lab on AWS environment with Amazon EC2's where password auhencation is disabled by default**

## Lesson 1: Install Ansible on RHEL. We will call this server Ansible-master

1) Download the EPEL repo
```console

rpm -ivh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
```

2) Install Ansible

```console
yum install ansible -y
```

3) Edit the hosts file for ansible and add the slave Ip. 

```
sudo vi /etc/ansible/hosts
```
4) Inside the file add these 2 lines. This give the group ( in this case only 1 instance ) a name . This is on top of the IP of the slave enclosed in the square brackets.
```vi
[webservers]
10.0.0.5
```


## Lesson 2: On another RHEL/centos machine which we will call Ansible-slave

- Add a user named ansible

```
sudo useradd ansible
```

- Add a password to the user and choose a password of your choice
```
sudo passwd ansible
```

- Enable password authentication. To do this open tis file
```
sudo vi /etc/ssh/sshd_config 
```

- Then uncomment the line that says PasswordAuthentication yes and comment the line that says PasswordAuthentication no :
```vi
# To disable tunneled clear text passwords, change to no here!
PasswordAuthentication yes
#PermitEmptyPasswords no
#PasswordAuthentication no
```

- Restart the sshd daemon on the Slave machine 
```
sudo service sshd restart
```

- Try logging into the slave machine from the Ansible master. you should be able to 

``` 
ssh ansible@10.0.0.5 
```



