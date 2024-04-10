# Argo CD Workflows

[Argo Workflow Quick Start](https://argo-workflows.readthedocs.io/en/latest/quick-start/)

## Install

```bash
kubectl create namespace argo
kubectl apply -n argo -f https://github.com/argoproj/argo-workflows/releases/download/v3.5.5/quick-start-minimal.yaml
```

## Argo CLI

### Execute Tests

We can submit workflows via the Argo CLI

```bash
argo submit -n argo --watch tests/hello-world.yaml
```

### View Results

```bash
argo list -n argo
argo get -n argo @latest
argo logs -n argo @latest
```

## Argo UI

```bash
kubectl -n argo port-forward service/argo-server 2746:2746
```

Navigate to https://localhost:2746

Click + Submit New Workflow and then Edit using full workflow options.

## Summary

Argo Workflows seems like a very strong choice. It was very easy to set up and install and seems very light weight.

It has both CLI and UI options. The UI option has an editor for creating workflows from scratch. From the CLI we can specify multiple files, so many workflows can be run at once. Workflows can be run in response to events, so we could create custom events to run a set of workflows to act as a test suite.

There are also many examples at https://github.com/argoproj/argo-workflows/blob/main/examples/, which may be of use.

Inputs can be passed to multiple steps, as well as outputs from previous steps.

Any image may be used for any step. This will allow us to define the steps, potentially mount script files from config maps in our preferred language, and execute them sequentially or in parallel.

We can also define templates, which may come in handy for similar tests, although I have not fully investigated that.

### Scoring

1. How easy is it to create declarative, arbitrary tests? Consider defining dependencies and passing arguments between stages.

   - Tests can be declared as yaml manifests
   - Any image can be used, and command can be set inline or mounted via file/config
   - UI can be used to create manifest
   - Examples can be found online

   4/5

2. How easy is it to execute individual or groups of tests?

   - Tests can be executed via Argo CLI
   - Tests can be executed via Argo UI
   - Multiple tests can be run with one CLI command
   - Workflows can call workflows, and could be used to create test suites

   4/5

3. How easy is it to view the results of tests? Consider live tests and previous results.

   - View results from CLI
   - View results from UI
   - Easily view latest results

   4/5

4. How easy was installation of the framework? How many resources does it use?

   - One line

   5/5
