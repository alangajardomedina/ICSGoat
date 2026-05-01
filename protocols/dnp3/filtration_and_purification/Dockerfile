FROM python:3.8

WORKDIR /app

RUN apt-get update && apt-get install -y \
    git \
    cmake \
    build-essential \
    g++ \
    python3-dev \
    libssl-dev \
    libffi-dev \
    libasio-dev \
    libboost-all-dev \
    automake \
    libtool \
    pkg-config \
    curl

RUN git clone --recursive https://github.com/Kisensum/pydnp3

RUN cd pydnp3 && \
    git submodule update --init --recursive && \
    python3 setup.py install

RUN rm /app/pydnp3/examples/outstation.py
COPY ./plc /app/pydnp3/examples

COPY entrypoint.sh /app/entrypoint.sh

# 🔥 FIX real
RUN sed -i 's/\r$//' /app/entrypoint.sh && \
    chmod +x /app/entrypoint.sh

RUN pip install requests

EXPOSE 20000

# 👇 ejecutar explícitamente con shell
CMD ["/bin/bash", "/app/entrypoint.sh"]