#------Build Stage--------#

FROM python:3.12-slim-bookworm AS builder


WORKDIR /usr/src/app


COPY requirements.txt ./

RUN pip install --upgrade pip && \
    pip install --no-cache-dir -r requirements.txt

COPY . .


#------Release Stage--------#

FROM gcr.io/distroless/python3:nonroot

# Copy app
COPY --from=builder /usr/src/app/recommendation_server.py /usr/src/app

# Copy installed Python packages
COPY --from=builder /usr/local/lib/python3.12 /usr/local/lib/python3.12


WORKDIR /usr/src/app

USER nonroot

ENTRYPOINT [ "python3", "recommendation_server.py" ]

