# official Ubuntu as a parent image
FROM ubuntu:20.04

ENV DEBIAN_FRONTEND=noninteractive

# Update base system
RUN apt-get update && \
    apt-get install -y software-properties-common curl gnupg dos2unix bash && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Install Python 3.9
RUN add-apt-repository ppa:deadsnakes/ppa && \
    apt-get update && \
    apt-get install -y python3.9 python3.9-venv python3.9-dev python3-pip && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Install Node.js 20
RUN curl -fsSL https://deb.nodesource.com/setup_20.x | bash - && \
    apt-get install -y nodejs && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Set Python default
RUN update-alternatives --install /usr/bin/python python /usr/bin/python3.9 1 && \
    update-alternatives --install /usr/bin/pip pip /usr/bin/pip3 1

RUN python --version && node --version && npm --version

WORKDIR /app

# dnp3 client
RUN apt-get update && apt-get install -y git cmake g++
RUN git clone --recursive https://github.com/Kisensum/pydnp3
RUN cd pydnp3 && python setup.py install

COPY ./dnp3/filtration_and_purification /app/pydnp3/examples

# modbus client
RUN pip install pymodbus==3.6.6
COPY ./modbus/filling_and_packaging /app/filling_and_packaging
COPY ./modbus/raw_material_handling /app/raw_material_handling

# opcua client
COPY ./opcua/formulation /app/formulation
RUN cd /app/formulation && npm install

COPY ./opcua/fermentation /app/fermentation
RUN cd /app/fermentation && npm install

COPY ./opcua/mixing_and_blending /app/mixing_and_blending
RUN cd /app/mixing_and_blending && npm install

# database init
COPY ./database /app/db
RUN python /app/db/init_db.py

RUN pip install Flask flask-cors

COPY ./monitor /app/monitor

# mqtt
COPY ./mqtt /app/mqtt
RUN pip3 install --no-cache-dir paho-mqtt asyncua==0.8.4

# entrypoint
COPY entrypoint.sh /app/entrypoint.sh

# 🔥 FIX REAL (CRLF + permisos)
RUN dos2unix /app/entrypoint.sh && \
    chmod +x /app/entrypoint.sh

EXPOSE 26543

# 🔥 ejecución estable
CMD ["/bin/bash", "/app/entrypoint.sh"]