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

Tests are primarily triggered by events or are defined as periodic. Tests can either be added centrally as part of the config manifest (in _deploy/prow/config-maps.yaml_), or they can be scanned remotely as part of other repos from a _.prow_ directory, similar to _.github_ actions.

Some example tests are:

```yaml
periodics:
  - interval: 10m
    name: echo-test
    decorate: true
    spec:
      containers:
        - image: alpine
          command: ["/bin/date"]
postsubmits:
  YOUR_ORG/YOUR_REPO:
    - name: test-postsubmit
      decorate: true
      spec:
        containers:
          - image: alpine
            command: ["/bin/printenv"]
presubmits:
  YOUR_ORG/YOUR_REPO:
    - name: test-presubmit
      decorate: true
      always_run: true
      skip_report: true
      spec:
        containers:
          - image: alpine
            command: ["/bin/printenv"]
```

## Viewing Tests

Visit the $PROW_HOST from your local browser. Please note that if you may be required to add this host to your local hosts file for routing.

You can use the provided UI to navigate around the tests and view their status and logs.

## Summary

Prow seems very specific to CI/CD pipelines, and specifically GitHub. It does not seem to have the flexibility we want to run individual tests, create test suites and interact with it from the CLI.

It is also quite a heavy package, containing many dependencies, and the documentation was not great in setting it up. It seems like a tool that is used by many but maintained by few.

### Scoring

1. How easy is it to create declarative, arbitrary tests? Consider defining dependencies and passing arguments between stages.

   - Tests are declared in a specific format of configmap
   - Any image can be used, and command can be set inline or mounted via file/config

   1/5

2. How easy is it to execute individual or groups of tests?

   - More geared towards event triggers
   - Not clear how to execute tests at will, either by CLI or GUI

   1/5

3. How easy is it to view the results of tests? Consider live tests and previous results.

   - View results from CLI
   - View results from UI

   3/5

4. How easy was installation of the framework? How many resources does it use?

   - Very complicated schemas
   - Insufficient documentation

   0/5
