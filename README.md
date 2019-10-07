# containers-infrastructure-charts
Helm charts repository

This repository contains chart definitions for 
- the common part
- app containers



### Chart repository

Additionally, it serves as a chart repository

```
- helm package container1
- mv container1-0.1.0.tgz repository
- helm repo index repository --url https://raw.githubusercontent.com/lejeunen/containers-infrastructure-charts/master/repository
```

Using Terraform, the chart can then be referenced as

```
data "helm_repository" "containers" {
  name = "containers"
  url  = "https://raw.githubusercontent.com/lejeunen/containers-infrastructure-charts/master/docs/"
}

resource "helm_release" "module1" {
  repository = "${data.helm_repository.containers.metadata.0.name}"
  chart = "container1"
  version = "0.1.1"
  ...
}
```

Note that it's considered best practice that chart+version is immutable. 
This is not enforced by this kind of repository.

### Changes in common 

When the common part is updated, modules need to be updated *individually* with

```
cd container1
helm dependency update
```

### Things to improve

- Handling change in common is annoying and should be improved somehow
