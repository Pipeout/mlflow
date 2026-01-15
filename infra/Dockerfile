FROM python:3.10-slim

WORKDIR /mlflow

RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    curl \
    git \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

RUN pip install --no-cache-dir \
   mlflow \
   psycopg2-binary \
   boto3 \
   gunicorn
   
EXPOSE 5000