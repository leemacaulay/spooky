FROM ghost:2.0.3
RUN apt-get update -y && \
    apt-get install -y mysql-client
RUN npm install ghost-storage-adapter-s3
RUN mkdir -p ./content/adapters/storage
RUN cp -r ./node_modules/ghost-storage-adapter-s3 ./content/adapters/storage/s3
COPY ./wait-for-it.sh /usr/local/bin/wait-for-it.sh
