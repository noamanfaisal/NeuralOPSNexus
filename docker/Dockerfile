FROM python:3.12-slim-bookworm

# 1. Install System Dependencies (The heavy lifting)
RUN apt-get update && apt-get install -y \
    build-essential python3-dev libffi-dev libssl-dev \
    libjpeg-dev libxslt1-dev libicu-dev libpq-dev \
    curl git && rm -rf /var/lib/apt/lists/*

# 2. Install Rust (Required to compile the synapse-rust bridge)
RUN curl https://sh.rustup.rs -sSf | sh -s -- -y
ENV PATH="/root/.cargo/bin:${PATH}"

# We don't COPY your code here anymore. 
# We only set the stage.
WORKDIR /app

# We still need the metadata to install dependencies
COPY pyproject.toml README.rst /app/

# Install the base dependencies so they are cached in the image
RUN pip install --upgrade pip setuptools wheel
RUN pip install --no-cache-dir -e ".[all]" || true 
# (The 'true' prevents it from failing since the source code isn't there yet)

# ENTRYPOINT ["/bin/bash", "-c"]