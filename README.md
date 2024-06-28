# deployment-configurations

## Purpose

This repository contains configurations for deployments on the GlueOps Platform.  The repository layers configurations and allows common configuration options to be cascaded across deployments that share common attributes, such as a common image for an application that is deployed to different environments.

## Directory Structure

The repository contains three, core directories:

```sh
deployment-configurations
├── apps
├── common
├── env-overlays
```

* **apps**: Contians configurations for each applications deployed on the GlueOps platform.
* **common**: Contains any configurations common to all deployments on the platform.  For example, if all images are deployed from ghcr, that configuration can be set at this level to enable DRY configuration files.
* **env-overlays**: Contains configurations that will be applied to a collection of environments, such as `prod` vs `nonprod`.

## `apps` Directory

The `apps` directory is used to define the applications deployed on the platform and the environments for which each application will be deployed.

In the below example, two applications are deployed on the platform: `front-end-antonios-tacos` and `back-end-antonios-tacos`.  Each of those applications is deployed into three environments: `prod`, `uat`, and `stage`.

Any configurations shared among all environments, such as the image repository being deployed from and the image pull policy, can be added to the `base-values.yaml` in the `base` directory.

Configurations unique to each environment, such as image tag, secrets, and port, are stored within the various directories contained within the `env` directory in a `values.yaml` file.

`previews` is a special directory that is used to configure ephemeral deployments based upon open pull requests are is covered in more detail in this [document](tbd).

The `example-full-config` directory contains a `prod` example of the entire configuration without using the layering structure found in other examples.

```sh
apps
├── front-end-antonios-tacos
│   ├── base
│   │   └── base-values.yaml
│   └── envs
│       ├── previews
│       ├── prod
│       ├── stage
│       └── uat
└── back-end-antonios-tacos
    ├── base
    │   └── base-values.yaml
    └── envs
        ├── previews
        ├── prod
        ├── stage
        └── uat
```

## `env-overlays` directory

The `env-overlays` directory contains directories referring to groups of environments and is used to configure items common to a set of environments, but not to all environments.  Each directory contains an `env-values.yaml` file, which contains these configurations.

```sh
env-overlays
├── nonprod
│   └── env-values.yaml
└── prod
    └── env-values.yaml
```

In the above example, values contained within `nonprod` apply to all applications in all of the nonprod environments of Antionios Tacos - which in this case include `stage` and `uat`.
The `prod` directory contains configurations which apply to all applications in the `prod` environment.

## Creating your `deployment-configurations` Repository

To bootstrap the `deployment-configurations` repository, [create a template from this repository](https://github.com/new?template_name=deployment-configurations&template_owner=GlueOps) in the github organization with the associated application code and replace the sample application names (`front-end-antonios-tacos` and `front-end-antonios-tacos`) with the names of the applications you'd like to deploy.
**Note:** The names of each directroy _must_ match the names of the associated application repositories which contain application code.

Add new directories under `apps` for each additional application to deploy.

Similarly, change the subdirectories under each application to reflect the environments for which each application will be deployed.  These directories are customizable to fit an organization's deployment goals.

For example, to deploy the applications `data-api` in `stage` and `prod` environments and `commerce-front-end` in `uat` and `prod` environments, The resulting directory structure would be as follows:

```sh
deployment-configurations
├── apps
│   ├── back-end-antonios-tacos
│   │   ├── base
│   │   │   └── base-values.yaml
│   │   └── envs
│   │       ├── previews
│   │       │   ├── common
│   │       │   │   └── values.yaml
│   │       │   └── pull-request-number
│   │       │       └── 1
│   │       │           └── values.yaml
│   │       ├── prod
│   │       │   └── values.yaml
│   │       ├── stage
│   │       │   └── values.yaml
│   │       └── uat
│   │           └── values.yaml
│   └── front-end-antonios-tacos
│       ├── base
│       │   └── base-values.yaml
│       └── envs
│           ├── previews
│           │   ├── common
│           │   │   └── values.yaml
│           │   └── pull-request-number
│           │       └── 1
│           │           └── values.yaml
│           ├── prod
│           │   └── values.yaml
│           ├── stage
│           │   └── values.yaml
│           └── uat
│               └── values.yaml
├── common
│   └── common-values.yaml
├── env-overlays
│   ├── nonprod
│   │   └── env-values.yaml
│   └── prod
│       └── env-values.yaml
```
