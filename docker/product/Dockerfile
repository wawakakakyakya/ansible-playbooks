FROM base_cent7:latest as product

RUN set -x \
    : "install packages" \
    && yum -y update \
    && yum install -y which wget openssh-server openssh-clients net-snmp net-snmp-utils sudo \
    && yum clean all

COPY sshd_config /etc/ssh/sshd_config

ARG USERNAME
ARG PASSWORD

RUN set -x \
    : "Add User" \
    && useradd ${USERNAME} -m

RUN set -x \
    : "setup ssh" \
    && mkdir /var/run/sshd \
    && echo "${USERNAME}:${PASSWORD}" | chpasswd \
    && echo "${USERNAME}    ALL=(ALL)       ALL" >> /etc/sudoers \
    && sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd \
    && ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key -N '' \
    && mkdir  /home/${USERNAME}/.ssh \
    && touch  /home/${USERNAME}/.ssh/authorized_keys \
    && chmod 600 /home/${USERNAME}/.ssh/authorized_keys \
    && chown -R ${USERNAME}:${USERNAME} /home/${USERNAME}/.ssh \
    && source /etc/profile \
    && systemctl enable sshd
# https://docs.docker.com/engine/examples/running_ssh_service/
# RUN sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config

COPY id_rsa.pub  /home/${USERNAME}/.ssh/authorized_keys

CMD ["/sbin/init"]
