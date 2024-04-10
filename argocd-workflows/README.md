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
argo submit -n argo --watch https://raw.githubusercontent.com/argoproj/argo-workflows/main/examples/hello-world.yaml
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
