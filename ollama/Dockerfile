# We want to add curl to the ollama image
FROM ollama/ollama:latest

# Install curl
RUN apt-get update && \
    apt-get install -y curl && \
    rm -rf /var/lib/apt/lists/*
