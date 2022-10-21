# Kustomize + Helm


The idea:

You use kustomize not to patch k8s resources, but kustomize plugin configs instead.

Example for the hello-world service:
- in [bases/hello-world/helm-base](bases/hello-world/helm-base) you have a kustomize base for a helm generator
- in [envs/hello-world/prod/helm-overay](envs/hello-world/prod/helm-overay) you kustomize the generator for the prod environment
- in [envs/hello-world/prod](envs/hello-world/prod) you use the kustomized generator

- in [bases/hello-world/base](bases/hello-world/base) you have a kustomize base to transform resources
- in [envs/hello-world/prod](envs/hello-world/prod) you use the transformer to transform the generated helm resources


The following is a conceptual diagram to explain how the wiring is
happening in [envs/hello-world/prod](envs/hello-world/prod):

```
bases/hello-world  |    envs/hello-world/prod
                   |
helm-base -------> | --> helm-overay ---\
                   |                     \
                   |                      \-generate---> transform --> generated.k8s.yaml  
                   |                                  /
base ------------> | ----(optional overlay)--------- /
```

