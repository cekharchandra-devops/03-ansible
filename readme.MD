# Configuration Management

creating plain server without anything installed to preparing it and ready to serve the request to other applications or end user which includes 

* install application runtime
* create users and give proper executing permissions
* download the source code
* installing dependencies
* create systemctl service
* copy the config files
* run the service

disadvantages of using manully config the above steps or using shell script

* shell script not idempotent. we have write custom code to make it idempotent
* error handling. again we have to handle the exception and 
* homogenous 
* not scalable for many servers. we have to login to every server and run the script is not a good idea

To overcome above mentioned disadvanages, we have CM tools and Ansible is one of the popular

# PUSH VS PULL

Ansible primarily operates on a push model, but it can also be configured to mimic a pull model in some scenarios.

* In the push model, Ansible runs on a control node (usually your laptop or a central server) and directly executes tasks on target nodes via SSH. It doesnt require any additional software to be installed on node.

* In the pull model, managed nodes periodically pull playbooks or configuration files from a central repository (e.g., Git) and execute them locally using ansible-pull.agents required to be installed on nodes.

Ansible works similar like below without manually login to server, it executes remotely.

```
ssh ec2-user@<iP-addr> -C "echo 'Hell0' > /tmp/Hello.txt"
```

Install Ansible on Ansible server 

```
dnf install ansible -y
```

* we need to test whether able to ssh from ansible server to ansible node

```
ansible -i 172.31.83.28, all -e ansible_user=ec2-user -e ansible_password=DevOps321 -m ping
```

install nginx on ansible node and start the service using adhoc command

```
ansible -i 172.31.83.28, all -e ansible_user=ec2-user -e ansible_password=DevOps321 -b -m dnf -a "name=nginx state=installed"

ansible -i 172.31.83.28, all -e ansible_user=ec2-user -e ansible_password=DevOps321 -b -m service -a "name=nginx state=started"
```

executing the above adhoc commands one after the other is error prone if there is network issues.

* configure these commands as modules and execute them against ansible nodes is nothing but playbooks