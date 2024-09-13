FROM debian:stable-slim

# install deps
RUN apt-get update && \
    apt-get install -y aria2 && \
    rm -rf /var/lib/apt/lists/*

CMD ["aria2c", "--conf-path=/etc/aria.conf"]
