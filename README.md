# 1 - Create SSH key for Machines
```
ssh-keygen -t rsa -N "" -f ~/.ssh/id_rsa

cp id_rsa.pub authorized_keys

rename id_rsa private-key.pem
```

# 2 - build images
### cd server
```
docker build -t ubuntu-ansible-server .
```

### cd machines
```
docker build -t ubuntu-ansible-machine .
```

# 3 - Create a new network based on bridge

```
docker network create -d bridge somenetwork
```

# 4 - Run 1 container for server and 2 containers for machines
```
docker run -itd --network=somenetwork --name main-ansible ubuntu-ansible-server

docker run -itd --network=somenetwork --name machine-ansible-1 ubuntu-ansible-machine
docker run -itd --network=somenetwork --name machine-ansible-2 ubuntu-ansible-machine
```

# 5 - docker network inspect somenetwork (get ip)
```
docker network inspect somenetwork

docker ps -a --format "table {{.Names}}" | grep machine
docker network inspect somenetwork | grep '"IPv4Address"'

docker network inspect somenetwork --format "table {{.Containers}}" | grep '"IPv4Address"'
docker network inspect somenetwork --format  --format='{{range .NetworkSettings.Networks}}'
```

# 6 - Edit hosts on Server with ip
```
nano etc/ansible/hosts
```
add machine ip <172.18.0.3>

# 7 - How to test? Log in server and try execute for machines
```
ssh -i private-key.pem root@172.18.0.3
ansible all -a "/bin/echo hello" --private-key private-key.pem

ansible-playbook playbook.yml --private-key private-key.pem
```