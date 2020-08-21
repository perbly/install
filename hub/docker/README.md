Install Onify Hub in Docker
===========================

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

## Onify License

Get your Onify docker license key from hello@onify.co and add it as an environment variable in an `.env` file

```
LICENSE=<license key>
```

# Start Onify Hub in docker

```sh
docker-compose up
```

# Reset Hub

To reset the entire hub you can delete the indices and restart the api:


