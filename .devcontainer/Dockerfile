FROM python:3.11-slim

# Install system dependencies (curl, unzip, etc.)
RUN apt-get update && apt-get install -y \
    curl \
    unzip \
    && rm -rf /var/lib/apt/lists/*

# Install DuckDB CLI (latest static binary from GitHub)
RUN curl -L https://github.com/duckdb/duckdb/releases/download/v1.2.2/duckdb_cli-linux-amd64.zip -o duckdb.zip \
    && unzip duckdb.zip -d /usr/local/bin \
    && chmod +x /usr/local/bin/duckdb \
    && rm duckdb.zip

# Set working directory
WORKDIR /app

# Install Python dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy app code
COPY . .

# Default entry point (can be overridden)
CMD ["python", "examine-sample.ipynb"]
