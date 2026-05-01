FROM ubuntu:20.04

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && \
    apt-get install -y software-properties-common curl gnupg && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN add-apt-repository ppa:deadsnakes/ppa && \
    apt-get update && \
    apt-get install -y python3.9 python3.9-venv python3.9-dev python3-pip && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN curl -fsSL https://deb.nodesource.com/setup_20.x | bash - && \
    apt-get install -y nodejs && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN update-alternatives --install /usr/bin/python python /usr/bin/python3.9 1 && \
    update-alternatives --install /usr/bin/pip pip /usr/bin/pip3 1

WORKDIR /app

# ✅ FIX: mantener estructura correcta
COPY server /app/server

WORKDIR /app/server
RUN npm install

WORKDIR /app
COPY . .

# 🔥 FIX clásico CRLF
RUN sed -i 's/\r$//' /app/server/entrypoint.sh && \
    chmod +x /app/server/entrypoint.sh

EXPOSE 26543

CMD ["/bin/bash", "/app/server/entrypoint.sh"]