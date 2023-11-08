# Docker

- run bash in single-use debian container:
    - `docker run -it --rm debian bash`
- mount pwd in `/app`:
    - `docker run -it -v $(pwd):/app --rm debian bash`
- allow port 8080:
    - `docker run -it -v $(pwd):/app -p 8080:8080 --rm debian bash`

## Dockerfile
- allows (mostly) reproducible docker containers
- create docker container from dockerfile in current directory:
    - `docker build -t [TAG]`

```Dockerfile
# Dockerfile

FROM debian

RUN apt update
RUN apt install -y python3

WORKDIR /app

COPY server.py .
CMD ["python3", "server.py"]
```

```Dockerfile
# Dockerfile

FROM python

WORKDIR /app

COPY server.py .
CMD ["python3", "server.py"]
```

## Docker-compose
```yaml
# docker-compose.yaml

services:
  docker-demo:
    build: .
    ports:
      - "8080:8080"
```
