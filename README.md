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



# build images

# cd server
docker build -t ubuntu-ansible-server .

# cd machines
# https://linuxaws.wordpress.com/2017/07/17/how-to-generate-pem-file-to-ssh-the-server-without-password-in-linux/
docker build -t ubuntu-ansible-machine .

# run 2 containers

docker run -itd --network=somenetwork --name main-ansible ubuntu-ansible-server
docker run -itd --network=somenetwork --name machine-ansible ubuntu-ansible-machine


# start ssh each container
https://phoenixnap.com/kb/how-to-enable-ssh-on-ubuntu

service ssh start
systemctl enable ssh
service ssh status

# docker network inspect somenetwork
docker network inspect somenetwork

# edit hosts
nano etc/ansible/hosts
add machine ip <172.18.0.3>




ssh -i private-key.pem root@172.18.0.3
ansible all -a "/bin/echo hello" --private-key private-key.pem























# 1 - machine
ssh-keygen -t rsa -N "" -f ~/.ssh/id_rsa
cp id_rsa.pub authorized_keys
rename id_rsa private-key.pem

# 2 - build images
# cd server
docker build -t ubuntu-ansible-server .

# cd machines
docker build -t ubuntu-ansible-machine .

# 3 - run 2 containers
docker run -itd --network=somenetwork --name main-ansible ubuntu-ansible-server
docker run -itd --network=somenetwork --name machine-ansible ubuntu-ansible-machine

# 4 - docker network inspect somenetwork
docker network inspect somenetwork

docker ps -a --format "table {{.Names}}" | grep machine
docker network inspect somenetwork | grep '"IPv4Address"'

# 5 - edit hosts
nano etc/ansible/hosts
add machine ip <172.18.0.3>

# 6 - test
ssh -i private-key.pem root@172.18.0.3
ansible all -a "/bin/echo hello" --private-key private-key.pem