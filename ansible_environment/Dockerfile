FROM ubuntu:18.04 

RUN apt-get update \
    && apt-get install -y openssh-server curl python3 python3-pip \
    && apt-get clean all

# Latest versions of python tools via pip 
RUN pip3 install --upgrade pip virtualenv requests

RUN ln -sf /usr/share/zoneinfo/Europe/Berlin /etc/localtime

RUN mkdir /var/run/sshd \
   && mkdir /root/.ssh

RUN sed -ri 's/^PermitRootLogin\s+.*/PermitRootLogin yes/' /etc/ssh/sshd_config \ 
  && sed -ri 's/UsePAM yes/#UsePAM yes/g' /etc/ssh/sshd_config

COPY sshkey.pub /root/.ssh/authorized_keys

RUN chmod 400 /root/.ssh/authorized_keys

EXPOSE 22 

CMD ["/usr/sbin/sshd", "-D"]
