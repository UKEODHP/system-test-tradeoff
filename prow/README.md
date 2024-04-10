# Prow

## Installation

### GitHub Access

Create a GitHub app with the following permissions.

Repository permissions:

- Actions: Read-Only (Only needed when using the merge automation tide)
- Administration: Read-Only (Required to fetch teams and collaborators, Read & write needed when using branch protection automation)
- Checks: Read-Only (Only needed when using the merge automation tide)
- Contents: Read (Read & write needed when using the merge automation tide)
- Issues: Read & write
- Metadata: Read-Only
- Pull Requests: Read & write
- Projects: Admin when using the projects plugin, none otherwise
- Commit statuses: Read & write

Organization permissions:

- Members: Read-Only (Read & write when using peribolos)
- Projects: Admin when using the projects plugin, none otherwise

Subscribe to events:

- All events

After you saved the app, click “Generate Private Key” on the bottom and save the private key together with the App ID in the top of the page.

### Deploy Prow

First you need to populate an environment file, assumed _.env_, with the required variables. See _deploy/example.env_ for examples.

The HMAC_TOKEN is the token that you give to GitHub for validating webhooks. Generate it using any reasonable randomness-generator, e.g. `openssl rand -hex 20`.

Afterwards, edit your GitHub app and set Webhook secret to the HMAC_TOKEN value.

The GITHUB_TOKEN is the RSA private key, encoded as base64 with newlines stripped (`cat ./id_rsa | base64 -w 0`), and app id you created above for the GitHub App.

```bash
set -a  # automatically export variables
. deploy/.env  # source environment variables (and export them)
set +a  # stop automatically exporting variables
kustomize build deploy | envsubst | kubectl apply --server-side=true -f -  # substitute templated environment vars and apply manifests
```

From [prow-getting-started](https://docs.prow.k8s.io/docs/getting-started-deploy/).

## Running a Test

## Viewing Tests

Visit the $PROW_HOST from your local browser. Please note that if you may be required to add this host to your local hosts file for routing.

You can use the provided UI to navigate around the tests and view their status and logs.
