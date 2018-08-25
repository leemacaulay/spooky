FROM ghost:2.0.3
RUN apt-get update -y && \
    apt-get install -y mysql-client
COPY ./wait-for-it.sh /usr/local/bin/wait-for-it.sh
