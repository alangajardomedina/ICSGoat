FROM python:3.9-slim

RUN apt-get update && apt-get install -y procps bash

WORKDIR /app

COPY ./plc /app

RUN pip install --no-cache-dir -r requirements.txt

COPY entrypoint.sh /app/entrypoint.sh

# 🔥 FIX REAL
RUN sed -i 's/\r$//' /app/entrypoint.sh && \
    chmod +x /app/entrypoint.sh

EXPOSE 502

CMD ["/bin/bash", "/app/entrypoint.sh"]