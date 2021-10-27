FROM busybox:latest

RUN mkdir -p /usr/src/data && \
    touch save_file

VOLUME ["/usr/src/data"]

WORKDIR /usr/src/app
COPY entrypoint.sh .

CMD ["./entrypoint.sh", "../data/save_file"]