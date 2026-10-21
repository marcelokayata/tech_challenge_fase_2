# FROM python:3.13-alpine AS builder
# RUN adduser -D appuser
# USER appuser
# WORKDIR /app
# COPY --chown=appuser . .
# RUN pip install -r requirements.txt
# CMD ["python", "main.py"]

#--------------------------------------------

FROM python:3.13-slim AS builder

WORKDIR /build

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt


FROM python:3.13-slim AS runtime

WORKDIR /app

RUN useradd --create-home --no-log-init appuser
COPY --chown=appuser:appuser app.py .

USER appuser
EXPOSE 8005

CMD ["sh", "-c", "exec gunicorn --bind 0.0.0.0:${PORT:-8005} app:app"]