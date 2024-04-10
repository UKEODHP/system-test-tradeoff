# Tekton

## Installation

```bash
kubectl apply -f https://storage.googleapis.com/tekton-releases/pipeline/latest/release.yaml
kubectl get pods -n tekton-pipelines -w
```

## Run Test

```bash
kubectl apply -f tests/hello-world.yaml  # create test
kubectl apply -f tests/hello-world-run.yaml  # run test
kubectl get taskrun hello-task-run  # check test
kubectl logs --selector=tekton.dev/taskRun=hello-task-run  # check logs
```

## Run Pipeline

For the final command Tekton CLI `tkn` must be installed.

```bash
kubectl apply -f tests/goodbye-world.yaml  # create another task
kubectl apply -f tests/hello-goodbye-pipeline.yaml  # create a pipeline
kubectl apply -f tests/hello-goodbye-pipeline-run.yaml  # run pipeline
tkn pipelinerun logs hello-goodbye-run -f  # check logs
```

## Dashboard

### Installation

```bash
kubectl apply -f https://storage.googleapis.com/tekton-releases/dashboard/latest/release.yaml
```

### View Dashboard

```bash
kubectl port-forward svc/tekton-dashboard 9097:9097 -n tekton-pipelines
```

## Summary

Very easy to install. Tasks simple to understand and can easy scale to more complicated pipelines.

UI seems a bit basic, and does not offer the ability to execute tasks and workflows by default.

Support for declarative triggers.
