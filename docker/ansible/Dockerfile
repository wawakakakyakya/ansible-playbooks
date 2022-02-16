FROM base_cent7:latest as ansible

RUN set -x \
  : "install packages" \
  && yum -y update \
  && yum install -y openssh-server openssh-clients sudo ansible git \
  && yum clean all \
  && mkdir /root/.ssh

RUN ansible-galaxy collection install community.general ansible.posix

COPY id_rsa /root/.ssh/id_rsa
COPY id_rsa.pub /root/.ssh/id_rsa.pub
COPY config /root/.ssh/config

RUN chmod 600 /root/.ssh/config \
  && chmod 600 /root/.ssh/id_rsa

RUN mkdir -p /opt/ansible_playbooks
WORKDIR /opt/ansible_playbooks
ADD . /opt/ansible_playbooks

CMD ["/sbin/init"]
