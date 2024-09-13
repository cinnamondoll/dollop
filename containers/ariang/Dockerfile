FROM debian:stable-slim

# install deps
RUN apt-get update && \
    apt-get install -y wget unzip nginx && \
    rm -rf /var/lib/apt/lists/*

# version from build args
ARG version

# install ariang
RUN wget https://github.com/mayswind/AriaNg/releases/download/${version}/AriaNg-${version}.zip -O /tmp/ariang.zip && \
    unzip /tmp/ariang.zip -d /var/lib/ariang && \
    rm /tmp/ariang.zip

# aria config for rpc connections
COPY aria.conf /etc/nginx/conf.d/aria.conf

# disable default site
RUN rm /etc/nginx/sites-enabled/default

# remove ugly favicon
RUN rm /var/lib/ariang/favicon.ico /var/lib/ariang/favicon.png

CMD ["nginx", "-g", "daemon off;"]
