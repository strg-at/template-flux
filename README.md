<!-- markdownlint-disable MD041 -->
<!-- markdownlint-disable MD033 -->
<!-- markdownlint-disable MD028 -->
<!-- markdownlint-disable MD024 -->

<!-- PROJECT SHIELDS -->
<!--
*** I'm using markdown "reference style" links for readability.
*** Reference links are enclosed in brackets [ ] instead of parentheses ( ).
*** See the bottom of this document for the declaration of the reference variables
*** for contributors-url, forks-url, etc. This is an optional, concise syntax you may use.
*** https://www.markdownguide.org/basic-syntax/#reference-style-links
-->

[![pre-commit][pre-commit-shield]][pre-commit-url]
[![taskfile][taskfile-shield]][taskfile-url]

# Customer Flux Cluster

GitOps Kubernetes Cluster for Customer. Infrastructure as Code (IaC) with Flux2.

<details>
  <summary style="font-size:1.2em;">Table of Contents</summary>
<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

- [Code-Style](#code-style)
  - [Best practices](#best-practices)
  - [Naming](#naming)
- [Getting Started](#getting-started)
  - [Prerequisties](#prerequisties)
  - [Initialize repository](#initialize-repository)
- [Configuration](#configuration)
  - [Preparation](#preparation)
  - [Encryption with SOPS](#encryption-with-sops)
  - [Howto add resources](#howto-add-resources)
  - [Authentication and permission configuration](#authentication-and-permission-configuration)
- [Known Issues](#known-issues)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->
</details>

## Code-Style

### Best practices

[Kubernetes configuration best practices][kubernetes-best-practices]

### Naming

- lower-case characters
- hyphen

Pattern: `[a-z-]+`

## Getting Started

### Prerequisties

- [pre-commit][pre-commit-url]

### Initialize repository

pre-commit framework needs to get initialized.

```console
task pre-commit:init
```

## Configuration

### Preparation

All changes require a PR and review. Create a new branch and reference a Jira ticket, f.e.

```console
git switch -c feature/INPRO-1-configure-resource
```

### Encryption with SOPS

The cluster has SOPS encryption in place

To be able to decrypt secrets you need to have the role in Google Cloud assigned.

Addons like [@signageos/vscode-sops][@signageos/vscode-sops] for VSCode enable the IDE to automatically decrypt and open secrets in IDE.

#### How to create and update SOPS secrets

##### create file

```sh
sops test.devs.sops.yaml
```

This works without further parameters because the path_regex configured in `.sops.yaml` matches.
Be sure to add `# yamllint disable` to top and keep this comment unencrypted.

##### encrypt existing file

```sh
sops -e -i test.devs.sops.yaml
```

##### update encryption key of file

After updating the encryption key(s) in `.sops.yaml` - this counts for new verions in existing keys as well - run for each encrypted file:

```sh
sops updatekeys test.devs.sops.yaml
```

### Howto add resources

To keep the Kubernetes resources in sync with the source repository, [Flux][flux] is in place.

Whenever there is a push to mainline, Flux will reconcile the resources with the desired state defined by the repository. Be sure to go through the full QA cycle.

### Authentication and permission configuration

#### Flux

Flux reconciles the cluster recources and has a deploy key added to the repository to be able to read and update the repo items.

Additional, Flux has a Google Service account bound to the kustomize-controller Service account to be able to read the decryption key from Google KMS.
This enables Flux to decrypt secrets with SOPS. For more information how this works, check Flux [SOPS][flux-sops] documentation.

## Known Issues

<!-- TBD -->

<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->

<!-- Links -->

[kubernetes-best-practices]: https://kubernetes.io/docs/concepts/configuration/overview/
[@signageos/vscode-sops]: https://marketplace.visualstudio.com/items?itemName=signageos.signageos-vscode-sops
[flux]: https://fluxcd.io/flux/
[flux-sops]: https://fluxcd.io/flux/guides/mozilla-sops/

<!-- Badges -->

[pre-commit-shield]: https://img.shields.io/badge/pre--commit-enabled-brightgreen?logo=pre-commit
[pre-commit-url]: https://github.com/pre-commit/pre-commit
[taskfile-url]: https://taskfile.dev/
[taskfile-shield]: https://img.shields.io/badge/Taskfile-Enabled-brightgreen?logo=task
