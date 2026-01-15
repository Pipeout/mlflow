FROM python:3.10-slim


RUN apt-get update && apt-get install -y --no-install-recommends \
  build-essential \
  curl \
  git \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/*
  
WORKDIR /mlflow


RUN pip install --no-cache-dir \
  mlflow \
  psycopg2-binary \
  boto3 \
  gunicorn

EXPOSE 5000

CMD [ "mlflow", "server", "--host", \
  "0.0.0.0", \
  "--port", "5000"]
