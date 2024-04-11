# System Test Trade-off

## Issue

We wish to be able to run system tests. For example, such a test might create a workspace, link a git repo to it, run a workflow defined in the repo, check that catalogue entries produced by it appear in the catalogue and check the output data.

We also wish to be able to run smoke tests. These are non-disruptive tests which can run on a production platform, for example after a new deployment. These may also involve running fast-running test workflows in a test workspace, adding catalogue entries, etc.

We need a framework to run these in. We have previously used Testkube in EODHP-231: TestKube system environment test suites
DONE
but 1) it has been difficult to create tests declaratively rather than having to run CLI tools or use a UI, and 2) there seem to be some recent licence changes and it’s not clear if it’ll be possible to use a dashboard UI.

An alternative may be Jenkins X (which is different to Jenkins). There is also Prow, used by the Kubernetes project: Prow (Jenkins X includes Lighthouse, which is a Prow derivative).

From such a tool:

We want to run large, slow-running tests within a complete EODHP installation in a Kubernetes cluster.

These tests should be able to do and check things inside the cluster that are not necessarily exposed publicly, such as talking to Pulsar or changing Kubernetes resources.

We don’t want to run these as part of the build of one of our source repos. These tests will use all components of the system. However, they should still be defined in git repos, perhaps a dedicated one or multiple ones.

We should be able to trigger all of the tests easily, perhaps with a button press or command. We’ll do this with new releases in a test cluster.

We should be able to trigger tests selectively. They may be long-running and we don’t want to run hours of tests to try out a change to just one.

We should be able to see the test results in a convenient way, such as a screen in a UI. We want to be able to run all tests after a release to a test cluster and quickly see if the run succeeded.

These tests will most likely number in the tens rather than hundreds or thousands.

We want to know which tool(s) to use and what we should do to get started with it. This tool may still be Testkube if we can get it to meet the above needs.

## Scoring

Key questions will be taken from the trade-off issue description and asked of each option. Each question will be given a score out of 5. The scores will help make a decision between the different technologies.

### Questions

1. How easy is it to create declarative, arbitrary tests? Consider defining dependencies and passing arguments between stages.
2. How easy is it to execute individual or groups of tests?
3. How easy is it to view the results of tests? Consider live tests and previous results.
4. How easy was installation of the framework? How many resources does it use?

## Result

I believe that either Argo Workflows or Tekton would be sufficient for our use cases. We should select Argo Workflows since we already use ArgoCD for GitOps, but either will work with it fine according to suggestions online.

Prow is too intrinsically linked to the concept of CI/CD pipelines being built around event hooks for pull requests. It also seems quite specific to GitHub, which makes it less transferrable.

JenkinsX was not considered as a whole because it overlaps too much with the functionality of ArgoCD, which we are happy with.

TestKube seems to have the feature we want, but it is only available with the paid Pro version. It also seems quite an immature project compared with ArgoCD/Tekton, the documentation and examples online are lacking somewhat.
