Part 1 Create 3 ubuntu VM’s or Docker container 

    Step 1 Go to docker playground create an  instance / node and create 3 Ubuntu  container 

    1st --name ansible_master 
    2nd --name target1
    3rd --name target2

    Step 2 Update the packages and install docker for creating containers     
    - apt-get update && apt install docker.io -y

    step -> Verify if Docker is installed successfully 
    - docker -v, docker version
        Docker version 27.3.1, build ce12230

    Step -> Start the 3 containers using ubuntu image, one as ansible master and other 2 as target1 and target2 

    - docker run -itd --name ansible_master ubuntu /bin/bash
    - docker run -itd --name target1 ubuntu /bin/bash
    - docker run -itd --name target2 ubuntu /bin/bash


    Step 3 Varify the name process(container) are up and running 
    - docker ps

Done 

Part 2 Install Ansible and requied tools in ansible_master container  

    Step 4 Execute ansible_master container,update ubuntu's packages and install ansible and required dependencies for ansible as container are lightweight

    - docker exec -it ansible_master bash
    - sudo apt update
    - sudo apt install python-is-python3 vim iputils-ping openssh-client -y
                              Geographical area 5 city 44
    - sudo apt install software-properties-common

    - add-apt-repository --yes --update ppa:ansible/ansible
    - apt install ansible

    Verify the ansible Installation
    - ansible --version
     
    Exit from ansible-master

------------

Part 3 Setup n number of target machines 

For Example -> 2 VM or 2 container, Login to Target machine and do setup of first machine named target1 

Step 5  Login to machine/container target1 
  - docker exec -it target1 bash

Step 6 Update and install SSH and required dependencies 
  - apt update
  - apt install vim python-is-python3 iputils-ping openssh-client -y
                          Geographical area 5 city 44
  - apt-get install openssh-client openssh-server -y


Step 7 edit sshd_config to allow SSH and root login as required by ansible-master to connect.

  - cd /etc/ssh 
  - vi sshd_config
Uncomment the parameter and modify the permission to yes
  - Port 22 
  - PermitRootLogin yes 
  - PasswordAuthentication yes

Step 8 Start the service ssh if its not running
  - service ssh status
  - service ssh start 
  - service ssh status  

Configuration Done For target machine named target1

Step 9 Change the root password to admin 
  - passwd root
  - admin
  - admin

---

Part 4 Perform the same all steps with second target machine named target2 and so on
Also we can make an image of above container and use that iamge to run the replica containes if required any number of machine

Step 10 
  - docker commit

Part 5  Find the IP’s of all targets container  for adding in ansible host file 
Come out to docker node and run the command 

Step 11
  - sudo docker inspect target1
  - sudo docker inspect target2 


You will find the IPAddress like 172.17.0.2 , 172.17.0.3

Done 

-------------

Part 6 Setup ansible_master for ssh connectivity  and adding IP’s in hostfile

Step 12  Go to ansible_master

  - docker exec -it ansible_master bash

Step 13 - Edit ansible host file of ansible-master and provide the target machines IP’s

  - cd /etc/ansible/
  - ls
  - vi hosts

  Go to hosts file Edit and Just provide the IP of the target machines like 
  - 172.17.0.2
  - 172.17.0.3

Step 14 - Verify if you are able to ping target machine from ansible_master

- ping <Target machine IP>

---
---
Setup the ssh connection from the ansible-master machine to all the target machines

Generate the SSH Key from the master machine 
- ssh-keygen
- -y
- pasword ENTER

Part 7 - Copy the generated ssh keys from ansible_master to target machine and check the connectivity

Step 15 Copy and move the generated key from ansible_master to remote target 

- ssh-copy-id root@172.17.0.2
- yes

Number of key (s) added: 1,

Done

Step 16   You should be able to connect to target machine as root user from ansible_master using 

- ssh root@172.17.0.2

Now We are in the target machine one named target1

verify nginx is there or not 

- service nginx status

Part 8 Now we will create a ansible playbook and run ansible-playbook for installing nginx on target machine from ansible-master machine

Step 17 :- Create a installnginx.yaml file inside ansible dir

- cd /etc/ansible
- vi installnginx.yaml

-> Paste this yml script
```yml
---
- hosts: all
    tasks:
      - name: Installing nginx of the latest version for ansible demo
        apt: name=nginx state=latest
```
Step 18: Run ansible playbook if only one host file is there with all the target machins IP's

- ansible-playbook installnginx.yaml 

Or Run ansible playbook if more then one host file is there with bunch of target machins IP's

- ansible-playbook -i hostFileName installinginx.yaml

```yml
  ---
  - hosts: all
      tasks:
        - name: Installing nginx latest and running the nginx
          apt: name=nginx state=latest
            name: Making nginx Active or Running
            service: 
              name: nginx
              state: started
```
---
---
We can also uninstall the nginx from the target machine by running the playbook 
Again

- cd /etc/ansible
- vi uninstallnginx.yaml

```yml
  — hosts: all
      tasks:
        — name: UnInstall nginx 
          apt: name=nginx state=absent
```
Or 

```yml
- hosts: all
  tasks:
    - name: stop the runnig nginx
      service:
        name: nginx
        state: stopped
    - name: Uninstall the stoped nginx and ensune nginx is not installed
      apt: name=nginx state=absent
```
-> Run ansible playbook if only one host file is there with all the target machins IP's

- ansible-playbook uninstallnginx.yaml 

----------------------------------------------
https://github.com/DevSecOpsG/ansibleplaybookwithroles

https://github.com/geerlingguy/ansible-for-devops

https://github.com/ansible/ansible-examples

https://www.middlewareinventory.com/blog/ansible-ad-hoc-commands/

---
---
- hosts: dbservers
  tasks:
    - name: rmive nginxcccc
      apt: name=nginx state=absent
      
      
      ansible-playbook playbookcode.yaml

ansible-playbook -i host50 installnginx.yaml

mail.example.com

[webservers]
foo.example.com
bar.example.com

[dbservers]
one.example.com
two.example.com
three.example.com



calling roles 

---
- hosts: 10.2.2.1
  roles:
    - openjdk8
    
    -----
    
---
- name: Update all packages to the latest version
  become: true
  apt:
    upgrade: dist

- name: Install jenkins
  become: true
  apt:
    name: jenkins
    state: latest
    
    
ansible-playbook ansibleplaybookwithroles/openjdk8.yml


--------

maven 

---
- hosts: localhost
  roles:
    - role: maven
  vars_prompt:
    name: maven_version
    prompt: 'Choose your Maven version'
    default: '3.6.3'
    private: no
    
    

ansible-galaxy init jenkinsrole
