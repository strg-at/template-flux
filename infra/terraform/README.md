<!-- markdownlint-disable MD041 -->
<!-- markdownlint-disable MD033 -->
<!-- markdownlint-disable MD028 -->
<!-- markdownlint-disable MD024 -->

# Terraform Cluster infra

:warning: DO NOT add Terraform resources here that do not belong to the cluster. Use the terraform-infra repo instead. :warning:
:warning: DO NOT add Google IAM roles on project level here, it could interfere with already defined resources in the terraform-infra repo. :warning:

This terraform configuration contains all resources related to the cluster. It is safe to destroy them alongside the cluster, other resources in Google Cloud will not be affected.

## Terraform docs

<!-- prettier-ignore-start -->
<!-- BEGIN_TF_DOCS -->

<!-- END_TF_DOCS -->
<!-- prettier-ignore-end -->
