# docker-compose 参考
```yaml
version: "2"
services:
  app1:
    build: ./python_app/
    ports:
      - "8080:8080"
  app2:
    build: ./python_app/
    ports:
      - "8081:8080"
  consul1:
    image: "progrium/consul:latest"
    container_name: "consul1"
    hostname: "consul1"
    ports:
      - "8400:8400"
      - "8500:8500"
      - "8600:53"
    command: "-server -bootstrap-expect 3 -ui-dir /ui"
  consul2:
    image: "progrium/consul:latest"
    container_name: "consul2"
    hostname: "consul2"
    expose:
      - "8400"
      - "8500"
      - "8600"
    command: "-server -join consul1"
    depends_on:
      - consul1
  consul3:
    image: "progrium/consul:latest"
    container_name: "consul3"
    hostname: "consul3"
    expose:
      - "8400"
      - "8500"
      - "8600"
    command: "-server -join consul1"
    depends_on:
      - consul1
```