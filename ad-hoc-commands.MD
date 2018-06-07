# ansible-practice

I'm assuming that all your servers have password ebabled.

## Step 1: Install ansible
sudo yum install ansible

## Step 2: Add a user ansible on the control machine
sudo useradd ansible

## Step 3: Add a password for the user ansible
sudo passwd ansible

## Step 4: Add the info for remote nodes and localhost on the /etc/ansible/hosts file

[webservers]\
172.31.15.67 user=ansible\
172.31.4.90  user=ansible\
localhost    user=ansible\

## Step 5: Open /etc/ansible/ansible.cfg and add this line:
host_key_checking = False


## Step 6: For all your remote nodes, repeat steps 2 and 3 and choose the same password for all machines


## Step 7 : From your control maching and ansible user . Ping all the remote nodes
```console
ansible all -m ping --ask-pass
```

## Step 8 : Install apache web server 
#become means that the ansible user will become root to perform installation
```console
ansible all -m yum -a "name=httpd state=present" --ask-pass --become
```

## Step 9: Start the apache web server
```console
ansible all -m service -a "name=httpd state=started" --ask-pass --become
```

## Step 10: Paster the public IP of your remote-machine on your browser. You should see the apache web server page.

# Steps 11 onwards is all optional.Do them if you want to run ansibe commands without entering the password everytime

## Step 11: On your control machine , generate a rsa key pair. Enter to select all the default opptions and don't choose a password for the key:
```console
ssh-keygen
```

## Step 12: Copy the public key to the remote nodes. Repeat this for each remote node
ssh-copy-id ansible@172.31.4.90

## Step 13: Ping the remote nodes without the password
```console
ansible all -m ping 
```

## You should be able to run ansible commands without the password now.





