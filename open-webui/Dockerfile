# We want to add curl to the open-webui image
# This is so we can preform health checks on the container
FROM ghcr.io/open-webui/open-webui:main

# Install curl
RUN apt-get update && \
    apt-get install -y curl && \
    rm -rf /var/lib/apt/lists/*
