# ansible-practice


# What's in this repo?

- Commands to install ansible on Red Hat Enterprise Linux (RHEL)
- A files with ad-hoc ansible commands to perform simple tasks like ping
- 2 playbooks that have 1 tasks each. 1 uses the copy module and 1 uses the shell module
- 1 playbook that calls a role that's contained in copy-role folder
- 1 html file that's used by the copy module



## Install Ansible on RHEL 

- Download the EPEL repo
```console

rpm -ivh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
```

- Install Ansible

```console
yum install ansible -y
```
