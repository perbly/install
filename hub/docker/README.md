Install Onify Hub in Docker
===========================

Use docker compose to run your hub.

# Images

To retreive images from the onify registry you need a registry secret. Contact Onify at hello@onify.co for this.

* hub-api
* hub-app
* agent-server

# Preparations

Some preparations is required to run in docker.

## Image registry secret

Onify images are stored in a registry that requires authentication to access.

Request your image registry secret from hello@onify.co and save as `keyfile.json` in this directory

Login to docker with:
```sh
cat keyfile.json | docker login -u _json_key --password-stdin https://eu.gcr.io
```

## Env file

Add a file named `.env` in the same directory. Prepare the file with environment variables required for the docker compose manifest:

```
ADMIN_PASSWORD=<administrator password>
API_CLIENT_SECRET=<hash secret>
API_APP_SECRET=<Onify hub app secret>
APP_API_TOKEN=
LICENSE=
```

- `ADMIN_PASSWORD`: administrator password
- `API_CLIENT_SECRET`: secret used to hash secrets
- `API_APP_SECRET`: hub app application secret for the api
- `APP_API_TOKEN`: [Api token](#api-token) used by app for backend communication
- `LICENSE`: [Onify license](#onify-license)

### Api-token

Token used to authenticate app backend with api. Generated api token from API_APP_SECRET:

```sh
source .env
echo "Bearer $(echo -n "app:$API_APP_SECRET" | base64)"
```

Add the result as `APP_API_TOKEN` value in `.env` file where the value is surrounded by quotes.

### Onify License

Get your Onify docker license key from hello@onify.co and add it to `LICENSE` variable in the `.env` file.

```
ADMIN_PASSWORD=<administrator password>
API_CLIENT_SECRET=<hash secret>
API_APP_SECRET=<Onify hub app secret>
APP_API_TOKEN="<Onify hub app api token>"
LICENSE=<Onify license>
```

# Start Onify Hub in docker

```sh
docker-compose up
```

or start things with names using `-d` to detach the container from the console.

```sh
docker-compose up -d elasticsearch
docker-compose up -d agent-server
```

# Reset Hub

To reset the entire hub you can delete the indices and restart the api:

```sh
curl -XDELETE http://localhost:9200/*
```

