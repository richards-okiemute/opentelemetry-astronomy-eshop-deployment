#------Build Stage--------#

FROM python:3.12-slim-bookworm AS builder


WORKDIR /usr/src/app


COPY requirements.txt ./

RUN pip install --upgrade pip && \
    pip install --no-cache-dir -r requirements.txt

COPY . .


#------Release Stage--------#

FROM python:3.12-alpine

WORKDIR /usr/src/app

# Copy everything from builder
COPY --from=builder /usr/src/app /usr/src/app

# Run as non-root

USER 1000:1000  

ENTRYPOINT ["python", "recommendation_server.py"]

