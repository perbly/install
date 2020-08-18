Install Onify Hub in kubernetes
===============================

# Images

To retreive images from the onify registry you need a registry secret. Contact Onify at hello@onify.co for this.

* hub-api
* hub-app
* agent-server

# Preparations

1. Get your container registry secret
2. Get your Onify license
3. Clone repository (optional)
4. Make sure container manifest labels are unique for your organization

# Deployment pipeline

1. [elasticsearch 7](#elasticsearch) container unless you have your own
2. Onify registration credentials (regcred-secret.yaml)
2. agent-server container
3. Setup hub-api container and environment variables
4. Setup hub-app container and environment variables
5. Optional [traefik](#traefic) container with external hosts and TLS termination

# Secrets

Secrets are saved in base64 format

`onify-regcred`: Registry credentials, contact Onify at hello@onify.co to get your credentials
`admin_password`: Admin password
`app_token_secret`: App token secret
`client_secret`: Client secret for hashing passwords and other secret information
`api_token`: Usually `Bearer app:<app_token_secret>` where `app:<app_token_secret>` also is base64 encoded

# Environment variables

## Environment variables for hub-api

- `ONIFY_adminUser_password`: from secret `admin_password`
- `ONIFY_adminUser_username`: administrator username, defaults to `admin`
- `ONIFY_apiTokens_app_secret`: from secret `app_token_secret`
- `ONIFY_autoinstall`: autoinstall Elasticsearch indices if they are missin
- `ONIFY_client_code`: Client three letter code, included in license
- `ONIFY_client_instance`: Client instance name, included in license
- `ONIFY_client_secret`: from secret `client_secret`
- `ONIFY_db_elasticsearch_host`: Elastic search host
- `ONIFY_db_indexPrefix`: Index name prefix, usually client code
- `ONIFY_initialLicense`: Optional, generated initial license if any
- `ONIFY_resources_baseDir`: Base directory for storing resource files, usually points to mounted folder
- `ONIFY_resources_tempDir`: Base directory for storing temporary files, usually points to mounted folder
- `ONIFY_websockets_agent_url`: Agent server websocket url

## Environment variables for hub-app

* `ONIFY_API_TOKEN` - Auth token for API. from secret `api_token`
* `ONIFY_API_URL_EXTERNAL` - External url for frontend API calls, should use `api_public_host`
* `ONIFY_API_URL_INTERNAL` - Internal url for backend connection with API, defaults to `http://api:8181/api/v2`, depends on exposed api service

# Elasticsearch

Onify persistance.

## `elasticsearch.yaml`

Elasticsearch manifest.

Configuration:
- `PersistentVolume`: Should be altered to match kubernetes installation
  - `hostPath.path`:Shared directory between

When applied the container can be tested by:

```sh
kubectl exec -it elasticsearch-0 -- curl http://localhost:9200
```

# Traefik

TLS termination requires certificates, the example manifest is configured to use letsencrypt.

Before installing traefik the following hosts have to be decided.

- `agent-server_host`: host for access to agent server (internal)
- `api_public_host`: host for public access to api
- `app_public_host`: host for public access to app

