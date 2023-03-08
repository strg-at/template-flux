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
[![Terraform][terraform-shield]][terraform-url]
[![Remote State file][statefile-shield]][statefile-url]

# Customer Flux Cluster

GitOps Kubernetes Cluster with Flux2 and Infrastructure as Code (IaC) with Terraform.

## Template introductions - remove me when initialized

- README.md: update h1
- README.md: update link to remote state file below
- .sops.yaml: update crypto key link for sops
- .github/workflows/terraform.yaml: update `env` and `runs-on`

See Confluence reference for Github Actions.

<details>
  <summary style="font-size:1.2em;">Table of Contents</summary>
<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

- [Code-Style](#code-style)
  - [Kubernetes](#kubernetes)
  - [Terraform](#terraform)
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

### Kubernetes

#### Best practices

[Kubernetes configuration best practices][kubernetes-best-practices]

#### Naming

- lower-case characters
- hyphen

Pattern: `[a-z-]+`

### Terraform

#### Best practices

[terraform-best-practices.com][terraform-best-practices]

#### Naming of Terraform resources

- lower-case characters
- underscores

Pattern: `[a-z_]+`

#### Naming of Google Cloud resources

- lower-case characters
- hyphen

Pattern: `[a-z-]+`

## Getting Started

### Prerequisties

- [terraform][terraform-url]
- [pre-commit][pre-commit-url]
- [terraform-docs][terraform-docs]
- [tflint][tflint]
- [yamllint][yamllint]
- [tfsec][tfsec]

### Initialize repository

Terraform and pre-commit framework need to get initialized.

```console
task terraform:init
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
touch test.sops.yaml
```

Be sure to add `# yamllint disable` to top and keep this comment unencrypted.

##### encrypt existing file

```sh
sops -e -i test.sops.yaml
```

##### update encryption key of file

After updating the encryption key(s) in `.sops.yaml` - this counts for new verions in existing keys as well - run for each encrypted file:

```sh
sops updatekeys test.sops.yaml
```

### Howto add resources

#### Kubernetes

To keep the Kubernetes resources in sync with the source repository, [Flux][flux] is in place.

Whenever there is a push to mainline, Flux will reconcile the resources with the desired state defined by the repository. Be sure to go through the full QA cycle.

#### Terraform

<!-- TBD -->

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
[terraform-best-practices]: https://www.terraform-best-practices.com/naming
[terraform-docs]: https://github.com/terraform-docs/terraform-docs
[tflint]: https://github.com/terraform-linters/tflint
[tfsec]: https://aquasecurity.github.io/tfsec
[yamllint]: https://github.com/adrienverge/yamllint
[@signageos/vscode-sops]: https://marketplace.visualstudio.com/items?itemName=signageos.signageos-vscode-sops
[flux]: https://fluxcd.io/flux/
[flux-sops]: https://fluxcd.io/flux/guides/mozilla-sops/

<!-- Badges -->

[pre-commit-shield]: https://img.shields.io/badge/pre--commit-enabled-brightgreen?logo=pre-commit&style=for-the-badge
[pre-commit-url]: https://github.com/pre-commit/pre-commit
[terraform-shield]: https://img.shields.io/badge/terraform-1.x-844fba?style=for-the-badge&logo=terraform
[terraform-url]: https://www.terraform.io/
[taskfile-url]: https://taskfile.dev/
[taskfile-shield]: https://img.shields.io/badge/Taskfile-Enabled-brightgreen?logoColor=white&style=for-the-badge&logo=data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAEAAAABACAYAAACqaXHeAAAEeElEQVR4Xu2bzU8TQRjGZ7atRQKaGAVRhBakNbYkGEPVE5IoJh403rxIgpgYwOBHPBgPxoTwDyiCelIx8SwqxhilEj9ihBCDhK8g0IIiEAqlgLi1YxZdU2DpzszOfhy2174z7/P+5nlntrtbCHT83BzqQRyAH6sc7v16yYB6JO6aCXX4Q+MFy3InR1Or0jwRrfVoCgAhZKkf7osmKrLK4dZUk2bJ7gT6UTQWw1pgCGCg0uHKxgpWGKQ6gM6Z0IPW0PgpGp1auEFVALeGehFN4SvHqAlCFQB3RwYQH03Y6sRcOABeVTjch4gHygxgCiD4c+5o09jIM9Yi4+fbPfjNVlxczIwuMwCs7I4Lj1VbKAbQOPqVD/O8FVc4yzgIwcPKbDfVBivqoAaAEDpYP9zXwrIg2rmUuIEKgNZ2xwEDOQ5UZuUR10M04PnUD/Q1PI2jR7eYJA6WlWe57uEKwAIwiFBS83DfAu6kRojDbQtZAEa0OwlgORBrAngyFmwL/JzfS5LMqLGpVq60NDOvUUqfJIDL3e3IuT7FqPVQ6ZqN8uDKTu+qeiUB7GxpWrqGv+h0UyUz2qAvs3837ob8fWQAhEEchOC8w2W0mrD0dM1Og/hfY1QAxEynM3PARpsNK7HeQYuxGOifC6+SoQiAOJvR20K0u9QiMAEgTJzEcaAiO0/vhV6WvzsyA36jxLcfmAEQM5/NygXJFl1+B/0vPopioCey2u6qOmDl5Hq1RSK7awpASLbBagPlO3I0aYveSBjwCO8Ga7wg5i0gVe0FpxvIXl8rwES66poDEBIKW9ElxhdRSgoXIWjigHjiuckp4Fj6dgXrDUBgYQ6Eo7yiOXQDICa+4HABCMkaQ3CRcCXH8qO5A2hPCxZ21/wUwF0pT0oqKNmyTTJ8ZGEOTDOyu2EBiMIuOXcB9O+nitAcnYztbngAgsAYQqBky1ZVj01djkHcdhDiDm9OJwlXFKv7Jiil3gRgOsBsAUV9TTLY3ANoboqSEKaJNTdBcxM0N0GazqEaY26C5iZI8WiMymsEg8xTwDwFDHgKCA4+0daKtLhJoUULWCBsqvP6jkt1puydSvFdAYK2JgpVFQCEoMHrS1ijLAChmgl+oejA25d+osowg9UCkD45b7uO8UotFgCxFs+bp0h49s7ywxqADXKDN7yF2M/oiACIhbNsC5YApK705BaLCoAw6f1A/9Wage5auQRy37MAcM7rTPXANKr/G1EDEAsrePcCRX4tytW55vdKAFgg/F7n9Uk/ZMBUpBiA0ragBUBjd6pjEBPkUljz+Oij6q72kyRjSAFsAjCrNt8XJMmRKJaZA+KTOF4/RlbMh6G4ACx2O6hzFTDXy3zCeBA4pwUOAFZ2V70FpBKc+fzB75+aKFrLhokAbLavK6tx7cF+9Z2mLVR1AI4bpABACGP1Xp+FpiDSMZoBEIS9R8H1pf6O+XiRKwGoaXddWkAqaXXnJ7558vvSC4YigAy7veKaq+A26QoqjdfUASvF5rY0oSNpGaDeU6ibjj9v/5xQDsUWRgAAAABJRU5ErkJggg==
[statefile-shield]: https://img.shields.io/badge/GCS-tfstate-F8991D.svg?style=for-the-badge&logo=googlecloud
[statefile-url]: https://console.cloud.google.com/storage/browser/customer-tf-states/customer-flux-cluster
