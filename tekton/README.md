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

### Scoring

1. How easy is it to create declarative, arbitrary tests? Consider defining dependencies and passing arguments between stages.

   - Tests can be declared as Task manifests
   - Multiple tasks can be run as pipelines
   - Any image can be used, and command can be set inline or mounted via file/config

   3/5

2. How easy is it to execute individual or groups of tests?

   - Additional manifest required to run Task
   - Tests can be executed via Tekton CLI
   - Multiple tests can be run with one CLI command
   - Pipelines can cal multiple tasks and pass arguments between them
   - Previous task run must be deleted before rerunning it

   2/5

3. How easy is it to view the results of tests? Consider live tests and previous results.

   - View results from CLI
   - View results from UI

   3/5

4. How easy was installation of the framework? How many resources does it use?

   - One line
   - Dashboard installed separately

   5/5
