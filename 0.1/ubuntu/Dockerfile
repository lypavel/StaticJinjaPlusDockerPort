FROM ubuntu:22.04

WORKDIR /opt/StaticJinjaPlus

ARG SJP_TAG=0.1.0
ARG SJP_HASH=0

RUN apt-get update && apt-get install -y \
python3 \
python3-pip \
python3.10-venv \
&& rm -rf /var/lib/apt/lists/*

ENV SJP_URL=https://github.com/MrDave/StaticJinjaPlus/archive/refs/tags/${SJP_TAG}.tar.gz
ENV SJP_FULLHASH=sha256:${SJP_HASH}

ADD --checksum=${SJP_FULLHASH} $SJP_URL /tmp/sjp.tar.gz
RUN tar xzf /tmp/sjp.tar.gz --strip-components=1 -C . && rm /tmp/sjp.tar.gz

RUN python3 -m venv venv && \
    venv/bin/pip install -r requirements.txt --no-cache-dir

ENTRYPOINT ["venv/bin/python3", "main.py"]
CMD ["--srcpath=./templates", "--outpath=./build"]