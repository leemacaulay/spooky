FROM ghost:2.0.3
RUN apt-get update -y && \
    apt-get install -y mysql-client
RUN cd /tmp && \
    npm install ghost-storage-adapter-s3 && \
    mkdir -p /var/lib/ghost/versions/$GHOST_VERSION/core/server/adapters/storage/ && \
    cp -r /tmp/node_modules/ghost-storage-adapter-s3 \
    /var/lib/ghost/versions/$GHOST_VERSION/core/server/adapters/storage/s3
COPY ./index.js /var/lib/ghost/versions/$GHOST_VERSION/core/server/adapters/storage/s3/index.js
COPY ./wait-for-it.sh /usr/local/bin/wait-for-it.sh
