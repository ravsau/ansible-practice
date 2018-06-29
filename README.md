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

```console
sudo vi /etc/ansible/hosts
```
4) Inside the file add these 2 lines. This give the group ( in this case only 1 instance ) a name . This is on top of the IP of the slave enclosed in the square brackets.
```vi
[webservers]
10.0.0.5
```
5) - Add a user named ansible

```console
sudo useradd ansible
```

- Add a password to the user and choose a password of your choice
```console
sudo passwd ansible
```

6) We will always work with ansible using the ansible user. So let's change users
```console
su ansible -
```



## Lesson 2: On another RHEL/centos machine which we will call Ansible-slave

- Add a user named ansible

```console
sudo useradd ansible
```

- Add a password to the user and choose a password of your choice
```console
sudo passwd ansible
```

- Enable password authentication. To do this open tis file
```console
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
```console
sudo service sshd restart
```

- Try logging into the slave machine from the Ansible master. you should be able to 

``` console
ssh ansible@10.0.0.5 
```

- Open the sudoers file on the slave machine
```console
sudo visudo
```

- Add this line. There is a line similar to this already for the wheel group . Put it right under it. This makes sure that the ansible user can call all commands without the need of password

```vi
ansible         ALL=(ALL)       NOPASSWD: ALL
```


- From the Ansible master ping the slave server:
```console
ansible -m ping all -u ansible --ask-pass
```

- you should see a return like this
```console
172.31.10.247 | SUCCESS => {
    "changed": false, 
    "ping": "pong"
}
```
**Congratulations , you are able to ping the slave server using the ping module!

- Now, let's install apache web server on the slave server. 
```console
ansible all  -m yum -a "name=httpd state=present" -u ansible --ask-pass --become

```

**In the above command -m stands for module , in this case we are using the yum module. -a is for arguements where we pass the the name of the package and state. present means install. Similarly if you run the command and say state=absent then the web server will be removed.  --become is used to be the root user with sudo

- Use the service module to start the apache web server

```console
ansible all  -m service -a "name=httpd state=started " -u ansible --ask-pass --become 

```

- Congratulations! your web server should be installed now. Browse the public IP of the slave server on a browser to verify. You should see the apache web server test page.

![apache web server](https://github.com/ravsau/ansible-practice/blob/master/images/Screen%20Shot%202018-06-28%20at%209.05.03%20PM.png)


