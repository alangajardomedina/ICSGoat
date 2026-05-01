FROM ubuntu:20.04

ENV DEBIAN_FRONTEND=noninteractive

# Dependencias base
RUN apt-get update && \
    apt-get install -y software-properties-common curl gnupg dos2unix && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Python
RUN add-apt-repository ppa:deadsnakes/ppa && \
    apt-get update && \
    apt-get install -y python3.9 python3.9-venv python3.9-dev python3-pip && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Node.js
RUN curl -fsSL https://deb.nodesource.com/setup_20.x | bash - && \
    apt-get install -y nodejs && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Python aliases
RUN update-alternatives --install /usr/bin/python python /usr/bin/python3.9 1 && \
    update-alternatives --install /usr/bin/pip pip /usr/bin/pip3 1

WORKDIR /app

# Copiamos TODO el proyecto una sola vez
COPY . /app

# Entramos al backend
WORKDIR /app/server

# Dependencias Node
RUN npm install

# 🔥 Fix típico de "no such file": CRLF + permisos
RUN dos2unix entrypoint.sh || true && \
    chmod +x entrypoint.sh

EXPOSE 26543

# Usar bash evita problemas de shebang
ENTRYPOINT ["bash", "/app/server/entrypoint.sh"]