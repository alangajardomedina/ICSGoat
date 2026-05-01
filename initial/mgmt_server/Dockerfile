FROM ubuntu:20.04

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y openssh-server supervisor

RUN mkdir /root/.ssh 

COPY johnny.pub /root/.ssh/authorized_keys

RUN chmod 700 /root/.ssh \
    && chmod 600 /root/.ssh/authorized_keys

COPY start.sh /usr/local/bin/start.sh

# 🔥 arregla CRLF + permisos
RUN sed -i 's/\r$//' /usr/local/bin/start.sh && \
    chmod +x /usr/local/bin/start.sh

CMD ["/bin/bash", "/usr/local/bin/start.sh"]