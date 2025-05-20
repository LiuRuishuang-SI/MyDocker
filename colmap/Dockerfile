FROM colmap/colmap:latest

RUN apt-get update && \
    apt-get install -y --no-install-recommends wget unzip && \
    rm -rf /var/lib/apt/lists/*

CMD ["colmap", "-h"]
