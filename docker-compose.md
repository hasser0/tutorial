# Commands
```sh
docker compose up [--detach] [--scale <service-name>=<n>]
docker compose down
docker compose stop
docker compose start
docker compose ls
```

```yaml
version: "3.7"

services:

  todo-web:
    image: diamol/ch06-todolist
    networks:
      - app-net
    depends_on:
      - todo-db

  todo-db:
    image: diamol/postgres
    ports:
      - "5433:5432"
    networks:
      - app-net
    volumes:
      - type: bind
        source:
    environment:
      - DATABASE=todolist
      - USER=u5erN4m3
      - PASSWORD=p455w0rd
    secrets:
        - db-secrets
        - source: root-secrets
          target: /

networks:
  app-net:
    name: nat
    external: true

secrets:
  db-secrets:
    file: <local-path>
  root-secrets:
    file: <local-path>

volumes:
  db-volume:
    name: "postgres-db"
    external: true
```
