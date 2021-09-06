FROM ubuntu:latest

RUN apt update 
RUN apt upgrade
RUN apt install iputils-ping -y
RUN apt install nano -y
RUN apt-get install openssh-server -y
# RUN apt-get install sshpass -y

COPY .ssh /root/.ssh
COPY sshd_config /etc/ssh/sshd_config

RUN chmod    700   ~/.ssh
RUN chmod 600 ~/.ssh/authorized_keys

RUN service ssh start
RUN systemctl enable ssh

EXPOSE 22

CMD ["/usr/sbin/sshd","-D"]
