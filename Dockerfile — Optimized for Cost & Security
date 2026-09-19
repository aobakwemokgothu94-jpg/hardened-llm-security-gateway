# --- Stage 1: Build & Dependencies ---
FROM python:3.11-slim AS builder

WORKDIR /app

# Prevent Python from writing .pyc files and enable unbuffered logging
ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1

RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    && rm -rf /var/lib/apt/lists/*

COPY requirements.txt .

# Install dependencies into a local directory to copy to the next stage
RUN pip install --no-cache-dir --user -r requirements.txt


# --- Stage 2: Final Secure Runtime ---
FROM python:3.11-slim AS runner

WORKDIR /app

ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1
ENV PATH=/root/.local/bin:$PATH

# Copy installed dependencies and application code from builder stage
COPY --from=builder /root/.local /root/.local
COPY app.py .

# 🛡️ Principle of Least Privilege: Avoid running as root in production
RUN useradd -u 10001 security_user && \
    chown -R security_user:security_user /app

USER security_user

EXPOSE 8080

# Run Uvicorn directly on port 8080 (Azure Container Apps default target port)
CMD ["uvicorn", "app:app", "--host", "0.0.0.0", "--port", "8080"]
