# Buildpack V3

This build template builds source into a container image using [Cloud Native Buildpacks](https://buildpacks.io).

The Cloud Native Buildpacks website describes v3 buildpacks as:

> ... pluggable, modular tools that translate source code into container-ready artifacts
> such as OCI images. They replace Dockerfiles in the app development lifecycle with a higher level
> of abstraction. ...  Cloud Native Buildpacks embrace modern container standards, such as the OCI
> image format. They take advantage of the latest capabilities of these standards, such as remote
> image layer rebasing on Docker API v2 registries.

**Note**: The current Cloud Foundry buildpacks are available in the [CF template](README-CF.md).

## Create the template

```
kubectl apply -f https://raw.githubusercontent.com/knative/build-templates/master/buildpacks/cnb.yaml
```

## Parameters

* **IMAGE:** The image you wish to create. For example, "repo/example", or "example.com/repo/image". (_required_)
* **BUILDER_IMAGE** The image on which builds will run (must include v3 lifecycle and compatible buildpacks; _required_)
* **USE_CRED_HELPERS:** Use Docker credential helpers. Set to `"true"` or `"false"` as a string value. (_default:_ `"false"`)
* **CACHE** The name of the persistent app cache volume (_default:_ an empty directory -- effectively no cache)
* **USER_ID** The user ID of the builder image user, as a string value (_default:_ `"1000"`)
* **GROUP_ID** The group ID of the builder image user, as a string value (_default:_ `"1000"`)

## Usage

```
apiVersion: build.knative.dev/v1alpha1
kind: Build
metadata:
  name: cnb-example-build
spec:
  source:
    git:
      url: https://github.com/my-user/my-repo
      revision: master
  template:
    name: buildpacks-cnb
    arguments:
    - name: IMAGE
      value: us.gcr.io/my-project/my-app
    - name: BUILDER_IMAGE
      value: cloudfoundry/cnb:bionic
    - name: CACHE
      value: my-cache
  volumes:
    - name: my-cache
      persistentVolumeClaim:
        claimName: my-volume-claim
```
