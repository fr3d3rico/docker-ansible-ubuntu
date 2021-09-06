FROM ubuntu:latest

RUN apt update
RUN apt upgrade
RUN apt install iputils-ping -y
RUN apt install ansible -y
RUN apt install nano -y
# RUN apt-get install openssh-server -y

COPY private-key.pem ./private-key.pem
COPY playbook.yml ./playbook.yml

RUN chmod 0400 private-key.pem

# RUN service ssh start
# RUN systemctl enable ssh

# EXPOSE 22

# CMD ["/usr/sbin/sshd","-D"]
