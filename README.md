# Ory Play

Quite interested in the tools here: <https://github.com/ory>. \
I'd like to try different authorization flows, and see how easy it is to add claims dynamically.

## Hydra

> **Warning**: This configuration uses static secrets to make it quick to spin up and not distract from the learnings. If you plan to use this for anything sensitive, change the way the secrets are supplied.

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

### Client Credentials Flow

In this flow, you provide login credentials, and are given an `access_token` with whcih to access the API (Resource Server) with.

This flow is sometimes called Machine-to-Machine (M2M) and is suitable for the following types of apps:
- CLIs
- daemons
- backend services

This flow is configured with the grant type `client_credentials`.

[Sequence Diagram][client-credentials-flow]

> **Note**: Most of these docker commands need specify the network so the host header matches. Otherwise, you can use the exposed port, but will need a hosts entry such as `127.0.0.1 hydra`.

#### Create a client

```sh
docker run --rm -it \
  --network "$(basename $(pwd))_ory" \
  oryd/hydra:v1.10.2 \
  clients create \
    --endpoint "http://hydra:4445" \
    --id "some-consumer" \
    --secret "some-secret" \
    --grant-types "client_credentials" \
    --response-types "token,code"
```

#### Issue an access token

> [Access token vs ID token?](https://oauth.net/id-tokens-vs-access-tokens)
> - Never read by the client (rather, the client shouldn't depend on the information in the token even if possible)
> - The resource server can read the token contents

> Notice the different port on the endpoint

```sh
TOKEN=$(docker run --rm -it \
  --network "$(basename $(pwd))_ory" \
  oryd/hydra:v1.10.2 \
  token client \
    --client-id "some-consumer" \
    --client-secret "some-secret" \
    --endpoint "http://hydra:4444" \
| tr -d '[[:space:]]')
```

Eg: `Wc6hdg690h5C_HWug-67EhREcXkRqOKD0rmnqi-31sc.bKWmvzy2zr8OsNjFYhKpZeBKUyzQLp3yLg2YS9t0x5M`

#### Validate the token

> Notice the different port on the endpoint

```sh
docker run --rm -it \
  --network "$(basename $(pwd))_ory" \
  oryd/hydra:v1.10.2 \
  token introspect \
    --endpoint "http://hydra:4445" \
    "$TOKEN"
```

Output:

```json
{
        "active": true,
        "aud": [],
        "client_id": "some-consumer",
        "exp": 1643454559,
        "iat": 1643450958,
        "iss": "http://127.0.0.1:9000/",
        "nbf": 1643450958,
        "sub": "some-consumer",
        "token_type": "Bearer",
        "token_use": "access_token"
}
```

---

## References

- [Run your own OAuth2 Server](https://www.ory.sh/run-oauth2-server-open-source-api-security/#performing-the-oauth2-client-credentials-flow)
- [ID Token and Access Token: What's the Difference?](https://auth0.com/blog/id-token-access-token-what-is-the-difference/)

[client-credentials-flow]: https://auth0.com/docs/get-started/authentication-and-authorization-flow/client-credentials-flow#how-it-works
