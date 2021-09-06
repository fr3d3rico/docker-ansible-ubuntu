apt-get install ansible

docker network inspect somenetwork

docker run -itd --network=somenetwork ubuntu
docker run -itd --network=somenetwork ubuntu

ansible-playbook main-remote.yml -i inventory --private-key ansible-key-pair.pem

# 
https://medium.com/trabe/use-your-local-ssh-keys-inside-a-docker-container-ea1d117515dc
https://phoenixnap.com/kb/generate-setup-ssh-key-ubuntu

ssh-keygen -E md5 -lf ~/.ssh/id_rsa.pub

# machine (frederico)
ssh-keygen -t rsa -b 4096 -f ./id_rsa_shared

ansible-playbook main-remote.yml -i inventory --private-key ansible-key-pair.pem

# build images

# cd server
docker build -t ubuntu-ansible-test1 .

# cd machines
# https://linuxaws.wordpress.com/2017/07/17/how-to-generate-pem-file-to-ssh-the-server-without-password-in-linux/
docker build -t ubuntu-ansible-slave .

# run 2 containers

docker run -itd --network=somenetwork --name main-ansible ubuntu-ansible-test1
docker run -itd --network=somenetwork --name slave-ansible ubuntu-ansible-slave


# start ssh each container
https://phoenixnap.com/kb/how-to-enable-ssh-on-ubuntu

service ssh start
systemctl enable ssh
service ssh status

# docker network inspect somenetwork
docker network inspect somenetwork

# edit hosts
nano etc/ansible/hosts
add slave ip <172.18.0.3>




ssh -i private-key.pem root@172.18.0.3
ansible all -a "/bin/echo hello" --private-key private-key.pem