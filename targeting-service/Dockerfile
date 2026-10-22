FROM python:3.13-slim AS builder

WORKDIR /build
RUN python -m venv /opt/venv
ENV PATH="/opt/venv/bin:$PATH"
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

FROM python:3.13-slim AS runtime

ENV PATH="/opt/venv/bin:$PATH" PYTHONDONTWRITEBYTECODE=1 PYTHONUNBUFFERED=1
WORKDIR /app
RUN useradd --create-home --no-log-init appuser
COPY --from=builder /opt/venv /opt/venv
COPY --chown=appuser:appuser app.py .
USER appuser
EXPOSE 8003
CMD ["sh", "-c", "exec gunicorn --bind 0.0.0.0:${PORT:-8003} app:app"]