# Ory Play

Quite interested in the tools here: <https://github.com/ory>. \
I'd like to try different authorization flows, and see how easy it is to add claims dynamically.

## Hydra

> On first run, run the hydra_migrate init service with the other hydra services disabled. Then flip it around.

```
docker compose up -d
docker compose logs hydra_migrate
```

> Now remember to comment out the `hydra_migration` init service (it is probably ok if it runs again, but no need right now).

### Test connectivity

```sh
curl -s http://127.0.0.1:9001/health/ready
```

You should get back an "ok" response:

```json
{"status":"ok"}
```
