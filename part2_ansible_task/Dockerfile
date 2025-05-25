FROM ubuntu:22.04

# Install SSH server and Python for Ansible
RUN apt-get update \
    && apt-get install -y openssh-server sudo \
    && rm -rf /var/lib/apt/lists/* \
    && useradd -m -s /bin/bash ansible \
    && echo 'ansible ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers \
    && mkdir -p /var/run/sshd /home/ansible/.ssh \
    && chown -R ansible:ansible /home/ansible

EXPOSE 22

CMD ["/usr/sbin/sshd","-D"]
