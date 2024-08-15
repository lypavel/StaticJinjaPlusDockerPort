FROM python:3.10-slim

WORKDIR /opt/StaticJinjaPlus

ADD https://github.com/MrDave/StaticJinjaPlus.git .

RUN python3 -m venv venv && \
    venv/bin/pip install -r requirements.txt --no-cache-dir

ENTRYPOINT ["venv/bin/python3", "main.py"]
CMD ["--srcpath=./templates", "--outpath=./build"]