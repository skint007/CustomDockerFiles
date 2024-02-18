# Custom dockerfiles
This is just a place for me to park my custom dockerfiles I use to customize other official images.

## docker-compose.yml example
```yml
services:
  ollama:
...
    container_name: ollama
    hostname: ollama
    #image: ollama/ollama
    restart: unless-stopped
    stop_grace_period: 30s
    build:
      context: https://github.com/skint007/CustomDockerFiles.git
      dockerfile: ollama/Dockerfile
...
```