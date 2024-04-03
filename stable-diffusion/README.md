This is based on Dockerfile's provided by [AbdBarho](https://github.com/AbdBarho/stable-diffusion-webui-docker)

### Example docker-compose for Portainer:

```yaml
x-base_service: &base_service
  environment:
    - PORT=7860
  volumes:
    - &v1 stablediffusion-data:/data
    - &v2 /opt/stablediffusion-output:/output
  stop_signal: SIGKILL
  tty: true
  networks:
    - reverseproxy-network
  deploy:
    restart_policy:
      condition: unless-stopped
      delay: 30s
      max_attempts: 3
      window: 120s
    resources:
      reservations:
        devices:
          - driver: nvidia
            count: all
            capabilities: [compute, utility, gpu]
services:
...
  sd-download:
    container_name: sd-download
    build:
      context: https://github.com/skint007/CustomDockerFiles.git
      dockerfile: stable-diffusion/download/Dockerfile
    volumes:
      - *v1

  sd-auto:
    <<: *base_service
    container_name: sd-auto
    hostname: sd-auto
    build:
      context: https://github.com/skint007/CustomDockerFiles.git
      dockerfile: stable-diffusion/AUTOMATIC1111/Dockerfile
    image: sd-auto:72
    environment:
      - PORT=7860
      - CLI_ARGS=--allow-code --medvram --xformers --enable-insecure-extension-access --api
  
  sd-invoke:
    <<: *base_service
    container_name: sd-invoke
    hostname: sd-invoke
    build:
      context: https://github.com/skint007/CustomDockerFiles.git
      dockerfile: stable-diffusion/invoke/Dockerfile
    image: sd-invoke:30
    environment:
      - PORT=7861
      - PRELOAD=true
      - CLI_ARGS=--xformers

  sd-comfy:
    <<: *base_service
    container_name: sd-comfy
    hostname: sd-comfy
    build:
      context: https://github.com/skint007/CustomDockerFiles.git
      dockerfile: stable-diffusion/comfy/Dockerfile
    image: sd-comfy:6
    environment:
      - PORT=7862
      - CLI_ARGS=
...
```