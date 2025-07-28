#------Build Stage--------#

FROM python:3.12-slim-bookworm AS builder


WORKDIR /usr/src/app


COPY requirements.txt ./

RUN pip install --upgrade pip && \
    pip install --no-cache-dir -r requirements.txt

COPY . .


#------Release Stage--------#

ENTRYPOINT ["python", "recommendation_server.py"]

