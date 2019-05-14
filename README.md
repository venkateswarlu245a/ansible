# ansible

Ansible
-------
1.what are all configuration management tools are available ?
2.what is ansible? why we choose ansible?
3.what is the purpose of ansible?
4.ansible vs chef ?
5.ansible architecture ?
6.ansible terminology ?
7.ansible installation  on aws ec2 
8.what is inventery file ?
9.what is ansible.conf file ?
10.what to divide the groups in inventory ?
11.add the nodes into ansible server?
12.run ping module to check whether nodes are connected or not ?

ansible
-------

configuration management tool and deployment tool

salt stack , cf engine, chef , puppet, ansible (infrastructure as a code )


install sofwares, up-gradation os, deployment,provisioning server

ansible is fast processing developed trough python 

yml

push mechanism 

inbuilt ssh 


ping modules

setup

shell
command
file
user
group
copy
git
yum
apt
template
inline
docker

ansible  nodes  -m copy  -a 'src=/etc/ansible/emexo.txt dest=/home/ec2-user/emexo.txt'

ansible nodes --private-key=tomcat.pem -m command -a 'pwd'

ansible nodes --private-key=tomcat.pem -m shell -a 'cat sample.txt| grep -i sample'

ansible nodes -m file --private-key=tomcat.pem -a 'dest=/home/ec2-user/emexo.txt mode=600'
ansible nodes -m file --private-key=tomcat.pem -a 'dest=/home/ec2-user/emexo_create.txt state=touch mode=644'
ansible nodes -m file --private-key=tomcat.pem -a 'dest=/home/ec2-user/emexo_create.txt state=absent'


nameofplaybook.yml

create file trough playbook
---------------------------
---
- hosts: nodes
  tasks:
  - name: Ansible create file if it doesn't exist example
    file:
      path: "/home/ec2-user/devops.txt"
      state: touch

	  
create_a_file.yml
	  
ansible-playbook create_a_file.yml --private-key=tomcat.pem --syntax-check
---
- hosts: nodes
  tasks:
  - name: Create file with content example
    copy:
      dest: "/home/ec2-user/devops.txt"
      content: |
        Madhu Sudhan Reddy
        Banglore
        DevOps Architech
		
ansible-playbook create_a_file_with_content.yml --private-key=tomcat.pem --syntax-check

create_a_multiple_files.yml
---
- hosts: nodes
  tasks:
  - name: Ansible create multiple files example
    file:
      path: "/home/ec2-user/{{ item }}"
      state: touch
      mode: 0775
    with_items:
    - access.log
    - systemd.log
    - properties.txt
    - conffile.conf
	

ansible-playbook create_a_multiple_files.yml -i /etc/ansible/hosts --private-key=tomcat.pem --syntax-check

---
- hosts: all
  tasks:
  - name: Ansible create multiple files example
    file:
      path: "/home/ec2-user/{{ item.location }}"
      state: touch
      mode: "{{ item.mode }}"
    with_items:
    - { location: 'task5_file1.txt',mode: '0566'}
    - { location: 'task5_file2.txt',mode: '0766'}


ansible nodes -m file --private-key=tomcat.pem -a 'dest=/home/ec2-user/sample.txt mode=642'

create a directory

ansible nodes -m file --private-key=tomcat.pem -a 'dest=/home/ec2-user/jmstechhome mode=600 state=directory'

to create a file

ansible nodes -m file --private-key=tomcat.pem -a 'dest=/home/ec2-user/testfile mode=600 state=touch'

To remove a File

ansible nodes -m file --private-key=tomcat.pem -a 'dest=/home/ec2-user/testfile mode=600 state=absent'


ansible nodes -b -m yum --private-key=tomcat.pem  -a 'name=httpd state=present'


ansible nodes -b -m service --private-key=tomcat.pem  -a 'name=httpd state=started'


ping
setup
copy
shell
command
file
LineInFile 
yum
service

ansible nodes --private-key=tomcat.pem -m lineinfile -a 'dest=/home/ec2-user/sample.txt  line="This notes written by madhu sudhan reddy"'


ansible 172.31.8.26 -b -m yum --private-key=tomcat.pem  -a 'name=git state=present'

ansible nodes -b -m service --private-key=tomcat.pem  -a 'name=httpd state=started'


172.31.8.26

ansible modules
--------------
create a file
writing content in file
change permission to file
create a directory
create a multiple files at a time using ansible loops
create a multiple files with different permissions using ansible loops
delete a file
real-time use-case example

agenda
-------
write a playbook to install git 

write a playbook to install maven

what is ansible roles

what is ansible-galaxy

how to create a role using anisble galaxy

write a playbook to install apache tomcat

deploy sample war file trough apache tomcat


git_install.yml
---
- hosts: nodes
  become: yes
  gather_facts: False
  tasks:	
    - name: Installing git
      yum:
        name: git
        state: present
---
- hosts: nodes
  become: yes
  tasks:
    - name: download maven and install
      shell: |
        wget http://repos.fedorapeople.org/repos/dchen/apache-maven/epel-apache-maven.repo -O /etc/yum.repos.d/epel-apache-maven.repo
        sed -i s/\$releasever/6/g /etc/yum.repos.d/epel-apache-maven.repo
        yum install -y apache-maven




ansible-playbook git_install.yml --syntax-check

ansible-playbook git_install.yml --private-key=tomcat.pem

ansible-playbook maven_install.yml --syntax-check

ansible-playbook maven_install.yml --private-key=tomcat.pem

ansible-playbook tomcat_setup.yml --syntax-check

ansible-playbook tomcat_setup.yml --private-key=/etc/ansible/tomcat.pem

https://github.com/ybmadhu/spring3-mvc-maven-xml-hello-world.git

ansible-playbook git_projects.yml --private-key=tomcat.pem
ansible-playbook git_projects.yml --private-key=tomcat.pem --skip-tags git_download
