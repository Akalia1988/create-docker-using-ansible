Project 1 
Create Jenkins,Ansible containers 


Project 2 
# create-docker-using-ansible
ansible automation on docker containers (AWS) 

create target containers and 1 ansible_master

docker run -itd --name target1 ubuntu /bin/bash

docker run -itd --name target2 ubuntu /bin/bash

docker run -itd --name ansible_master ubuntu /bin/bash


root@ip-172-31-84-73:~# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
520fbe4b22da        ubuntu              "/bin/bash"         7 seconds ago       Up 6 seconds                            ansible_master1
3ddbc0d08dda        ubuntu              "/bin/bash"         2 minutes ago       Up 2 minutes                            target2
df92c0186e49        ubuntu              "/bin/bash"         2 minutes ago       Up 2 minutes                            target1


lets go inside the containers and install what we be needed

docker exec -it ansible_master bash

root@ip-172-31-84-73:~# docker exec -it ansible_master1 bash
root@520fbe4b22da:/#

apt update

apt install python ansible vim iputils-ping openssh-client -y

lets go to our target containers now

docker exec -it target1 bash
apt update
apt install python vim iputils-ping openssh-server -y

in target host 

root@df92c0186e49:/# cd /etc/ssh
root@df92c0186e49:/etc/ssh# ls
moduli  ssh_config  ssh_host_ecdsa_key  ssh_host_ecdsa_key.pub  ssh_host_ed25519_key  ssh_host_ed25519_key.pub  ssh_host_rsa_key  ssh_host_rsa_key.pub  ssh_import_id  sshd_config
root@df92c0186e49:/etc/ssh# vim sshd_config

permitroot login change it to yes

now will reset the password of the root

root@df92c0186e49:/etc/ssh# passwd root
Enter new UNIX password:
Retype new UNIX password:
passwd: password updated successfully
root@df92c0186e49:/etc/ssh#
 service ssh restart

repeat same steps for other container too 

after

lets get the IP address of the target containers 
docker inspect target2

now I know the IP's I will go back to master container 

docker exec -it ansible_master1 bash
go to the host file

cd /etc/ansible

root@ip-172-31-84-73:~# docker exec -it ansible_master1 bash
root@520fbe4b22da:/# cd /etc/ansible
root@520fbe4b22da:/etc/ansible# ls -ltr
total 24
-rw-r--r-- 1 root root   982 Sep  5  2017 hosts
-rw-r--r-- 1 root root 19315 Apr 19  2018 ansible.cfg
root@520fbe4b22da:/etc/ansible# vim hosts




lets ping 

ping 172.17.0.2

 ssh-keygen

ssh-copy-id root@172.17.0.2



in a master container will write a playbook to install nginx 


create a file for the playbook

vim install_nginx.yml

Name: "nginx install & start services"
become: true
hosts: all
task: 
  - 
    Name: "install nginx"
    Yum: 
      name: nginx
      state: latest
  - 
    name: "start nginx"
    service: 
      name: nginx
      state: started










