FROM ubuntu:24.04

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
      bash \
      openssh-server \
      openssh-client \
      sudo \
      systemd \
      systemd-sysv \
      passwd \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

RUN useradd -m -s /bin/bash lab \
    && echo 'lab:lab' | chpasswd \
    && echo 'root:root' | chpasswd

RUN echo 'lab ALL=(ALL) NOPASSWD:ALL' > /etc/sudoers.d/lab \
    && chmod 0440 /etc/sudoers.d/lab

ENV container docker

COPY bash/.bashrc /home/lab/.bashrc
COPY bash/.bashrc /root/.bashrc
COPY scripts/entrypoint.sh /usr/local/bin/entrypoint.sh

RUN mkdir -p /etc/systemd/system/multi-user.target.wants \
    && chmod 600 /home/lab/.bashrc /root/.bashrc \
    && chown -R lab:lab /home/lab/.bashrc \
    && ssh-keygen -A \
    && sed -i 's/^#\\?PasswordAuthentication.*/PasswordAuthentication yes/' /etc/ssh/sshd_config \
    && sed -i 's/^#\\?PubkeyAuthentication.*/PubkeyAuthentication yes/' /etc/ssh/sshd_config \
    && sed -i 's/^#\\?PermitRootLogin.*/PermitRootLogin yes/' /etc/ssh/sshd_config \
    && grep -q '^PasswordAuthentication' /etc/ssh/sshd_config || echo 'PasswordAuthentication yes' >> /etc/ssh/sshd_config \
    && grep -q '^PubkeyAuthentication' /etc/ssh/sshd_config || echo 'PubkeyAuthentication yes' >> /etc/ssh/sshd_config \
    && grep -q '^PermitRootLogin' /etc/ssh/sshd_config || echo 'PermitRootLogin yes' >> /etc/ssh/sshd_config \
    && ln -sf /lib/systemd/system/ssh.service /etc/systemd/system/multi-user.target.wants/ssh.service \
    && ln -sf /lib/systemd/system/multi-user.target /etc/systemd/system/default.target

EXPOSE 22

ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]
CMD ["/sbin/init"]
