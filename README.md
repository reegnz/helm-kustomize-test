# Kustomize + Helm


The idea:

You use kustomize not to patch k8s resources, but kustomize plugin configs instead.

Example for the hello-world service:
- in [bases/hello-world/helm-base](bases/hello-world/helm-base) you have a
  kustomize base for a helm generator
- in [envs/hello-world/prod/helm-overlay](envs/hello-world/prod/helm-overlay)
  you kustomize the generator for the prod environment
- in [bases/hello-world/base](bases/hello-world/base) you have a kustomize base
  to patch resources (that got generated by helm)
- in [envs/hello-world/prod/overlay](envs/hello-world/prod/overlay) you can
  kustomize the patching
- in [envs/hello-world/prod](envs/hello-world/prod) you use the generator to
  generate the resources with helm, and the transformer to patch the generated resources.

The following is a conceptual diagram to explain how the wiring is
happening in [envs/hello-world/prod](envs/hello-world/prod):

```text
bases/hello-world  |    envs/hello-world/prod
                   |
helm-base -------> | --> helm-overay ---\
                   |                     \
                   |                      \-generate-----> transform --> generated.k8s.yaml  
                   |                                  /
base ------------> | --> overlay --------------------/
```


## Pitfalls

1. kustomize cannot seem to be able to handle using a base for a transformer like patchesStrategicMerge
   and taking the patch from a file in the base directory. This might be a kustomize bug
   that I need to report.
