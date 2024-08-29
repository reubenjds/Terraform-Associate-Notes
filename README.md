# HashiCorp Certified: Terraform Associate (003) Notes

1. [Understand Infrastructure as Code (IaC) Concepts](#1-understand-infrastructure-as-code-iac-concepts)
   - [1a. Explain what IaC is](#1a-explain-what-iac-is)
   - [1b. Describe advantages of IaC patterns](#1b-describe-advantages-of-iac-patterns)
2. [Understand the purpose of Terraform (vs other IaC)](#2-understand-the-purpose-of-terraform-vs-other-iac)
   - [2a. Explain multi-cloud and provider-agnostic benefits](#2a-explain-multi-cloud-and-provider-agnostic-benefits)
   - [2b. Explain the benefits of state](#2b-explain-the-benefits-of-state)
3. [Understand Terraform basics](#3-understand-terraform-basics)
   - [3a. Install and version Terraform providers](#3a-install-and-version-terraform-providers)
   - [3b. Describe plugin-based architecture](#3b-describe-plugin-based-architecture)
   - [3c. Write Terraform configuration using multiple providers](#3c-write-terraform-configuration-using-multiple-providers)
   - [3d. Describe how Terraform finds and fetches providers](#3d-describe-how-terraform-finds-and-fetches-providers)
4. [Use Terraform outside the core workflow](#4-use-terraform-outside-the-core-workflow)
   - [4a. Describe when to use terraform import to import existing infrastructure into your Terraform state](#4a-describe-when-to-use-terraform-import-to-import-existing-infrastructure-into-your-terraform-state)
   - [4b. Use terraform state to view Terraform state](#4b-use-terraform-state-to-view-terraform-state)
   - [4c. Describe when to enable verbose logging and what the outcome/value is](#4c-describe-when-to-enable-verbose-logging-and-what-the-outcomevalue-is)
5. [Interact with Terraform modules](#5-interact-with-terraform-modules)
   - [5a. Contrast and use different module source options including the public Terraform Module Registry](#5a-contrast-and-use-different-module-source-options-including-the-public-terraform-module-registry)
   - [5b. Interact with module inputs and outputs](#5b-interact-with-module-inputs-and-outputs)
   - [5c. Describe variable scope within modules/child modules](#5c-describe-variable-scope-within-moduleschild-modules)
   - [5d. Set module version](#5d-set-module-version)
6. [Use the core Terraform workflow](#6-use-the-core-terraform-workflow)
   - [6a. Describe Terraform workflow (Write -> Plan -> Create)](#6a-describe-terraform-workflow--write---plan---create-)
   - [6b. Initialize a Terraform working directory (terraform init)](#6b-initialize-a-terraform-working-directory-terraform-init)
   - [6c. Validate a Terraform configuration (terraform validate)](#6c-validate-a-terraform-configuration-terraform-validate)
   - [6d. Generate and review an execution plan for Terraform (terraform plan)](#6d-generate-and-review-an-execution-plan-for-terraform-terraform-plan)
   - [6e. Execute changes to infrastructure with Terraform (terraform apply)](#6e-execute-changes-to-infrastructure-with-terraform-terraform-apply)
   - [6f. Destroy Terraform managed infrastructure (terraform destroy)](#6f-destroy-terraform-managed-infrastructure-terraform-destroy)
   - [6g. Apply formatting and style adjustments to a configuration (terraform fmt)](#6g-apply-formatting-and-style-adjustments-to-a-configuration-terraform-fmt)
7. [Implement and maintain state](#7-implement-and-maintain-state)
   - [7a. Describe default local backend](#7a-describe-default-local-backend)
   - [7b. Describe state locking](#7b-describe-state-locking)
   - [7c. Handle backend and cloud integration authentication methods](#7c-handle-backend-and-cloud-integration-authentication-methods)
   - [7d. Differentiate remote state back end options](#7d-differentiate-remote-state-back-end-options)
   - [7e. Manage resource drift and Terraform state](#7e-manage-resource-drift-and-terraform-state)
   - [7f. Describe backend block and cloud integration in configuration](#7f-describe-backend-block-and-cloud-integration-in-configuration)
   - [7g. Understand secret management in state files](#7g-understand-secret-management-in-state-files)
8. [Read, generate, and modify configuration](#8-read-generate-and-modify-configuration)
   - [8a. Demonstrate use of variables and outputs](#8a-demonstrate-use-of-variables-and-outputs)
   - [8b. Describe secure secret injection best practice](#8b-describe-secure-secret-injection-best-practice)
   - [8c. Understand the use of collection and structural types](#8c-understand-the-use-of-collection-and-structural-types)
   - [8d. Create and differentiate resource and data configuration](#8d-create-and-differentiate-resource-and-data-configuration)
   - [8e. Use resource addressing and resource parameters to connect resources together](#8e-use-resource-addressing-and-resource-parameters-to-connect-resources-together)
   - [8f. Use HCL and Terraform functions to write configuration](#8f-use-hcl-and-terraform-functions-to-write-configuration)
   - [8g. Describe built-in dependency management (order of execution based)](#8g-describe-built-in-dependency-management-order-of-execution-based)
9. [Understand HCP Terraform capabilities](#9-understand-hcp-terraform-capabilities)
   - [9a. Explain how HCP Terraform helps to manage infrastructure](#9a-explain-how-hcp-terraform-helps-to-manage-infrastructure)
   - [9b. Describe how HCP Terraform enables collaboration and governance](#9b-describe-how-hcp-terraform-enables-collaboration-and-governance)

**Exam Format**: True/False, Multiple Choice, Multiple Answers, Short Answer (Text Match)

# Terraform Certification Notes

## 1. Understand Infrastructure as Code (IaC) Concepts

### 1a. Explain what IaC is

- **IaC (Infrastructure as Code)**: A practice where infrastructure is provisioned and managed using code instead of manual processes. It involves writing what you want to deploy in a human-readable form.
- **DevOps Enablement**: IaC is tracked in version control, allowing for better visibility and collaboration across teams.
- **Declarative Approach**: Infrastructure is defined declaratively via code, though it can also be procedural.

### 1b. Describe advantages of IaC patterns

- **Speed**: Automates deployment processes, saving time.
- **Cost Reduction**: Minimizes manual intervention, reducing errors and excess resource usage.
- **Reduced Risk**: Less human error means fewer security vulnerabilities.

## 2. Understand the purpose of Terraform (vs other IaC)

### 2a. Explain multi-cloud and provider-agnostic benefits

- **Multi-Cloud Support**: Terraform works with a variety of cloud providers, both public and private, making it provider-agnostic.
- **Unified Workflow**: Interacts with different cloud APIs using a consistent syntax.

### 2b. Explain the benefits of state

- **State Tracking**: Terraform maintains a state file (`terraform.tfstate`) to keep track of resource statuses. This is crucial for managing and updating resources effectively.
- **Deployment Planning**: Helps calculate changes needed and creates execution plans.

## 3. Understand Terraform basics

### 3a. Install and version Terraform providers

- **Installation**: Terraform can be installed by downloading the binary or using package managers. Providers are installed using `terraform init`.
- **Versioning**: Providers should be pinned to specific versions to avoid breaking changes.

### 3b. Describe plugin-based architecture

- **Providers**: Terraform uses plugins, known as providers, to interact with cloud platforms and other services.
- **Registry**: Providers are sourced from the Terraform providers registry or can be custom-built.

### 3c. Write Terraform configuration using multiple providers

- **Configuration**: Multiple providers can be declared in a Terraform configuration file to manage resources across different services.
- **Example**:

  ```hcl
  provider "aws" {
    region = "us-east-1"
  }

  provider "google" {
    region = "us-central1"
  }
  ```

### 3d. Describe how Terraform finds and fetches providers

- **Initialization**: During `terraform init`, Terraform downloads and installs the required providers.
- **Provider Source**: By default, Terraform looks in the Terraform providers registry.

## 4. Use Terraform outside the core workflow

### 4a. Describe when to use terraform import to import existing infrastructure into your Terraform state

- **Use Case**: When managing existing resources not initially created by Terraform. The `terraform import` command brings these resources into Terraform's management.

### 4b. Use terraform state to view Terraform state

- **Viewing State**: Commands like `terraform state list` and `terraform state show` allow users to inspect the current state file and the resources it manages.

### 4c. Describe when to enable verbose logging and what the outcome/value is

- **Verbose Logging**: Enabled by setting the environment variable `TF_LOG` to `TRACE`, `DEBUG`, etc., to provide detailed execution logs. This is useful for debugging and understanding Terraform's behavior.

## 5. Interact with Terraform modules

### 5a. Contrast and use different module source options including the public Terraform Module Registry

- **Sources**: Modules can be sourced from the public Terraform Module Registry, private registries, or local paths.
- **Example**:
  ```hcl
  module "network" {
    source = "terraform-aws-modules/vpc/aws"
    version = "2.0"
  }
  ```

### 5b. Interact with module inputs and outputs

- **Inputs**: Parameters passed to the module, used within the module as variables.
- **Outputs**: Values returned by the module to be used by the root module or other modules.

### 5c. Describe variable scope within modules/child modules

- **Scope**: Variables declared in the root module can be passed down to child modules. Variables are scoped to their respective modules unless explicitly passed.

### 5d. Set module version

- **Versioning**: Specifying a module version ensures compatibility and stability. Example:

  ````hcl
  module "network" {
    source  = "terraform-aws-modules/vpc/aws"
    version = "2.0.0"
  }

  ```markdown
  ````

## 6. Use the core Terraform workflow

### 6a. Describe Terraform workflow (Write -> Plan -> Create)

1. **Write**: Define infrastructure as code in `.tf` files.
2. **Plan**: Run `terraform plan` to preview changes.
3. **Apply**: Execute `terraform apply` to create/update infrastructure.

### 6b. Initialize a Terraform working directory (terraform init)

- Prepares the directory by downloading providers and modules.

### 6c. Validate a Terraform configuration (terraform validate)

- Checks the syntax and internal consistency of Terraform configuration files.

### 6d. Generate and review an execution plan for Terraform (terraform plan)

- Creates an execution plan showing what changes will be made.

### 6e. Execute changes to infrastructure with Terraform (terraform apply)

- Applies the changes as per the plan to the actual infrastructure.

### 6f. Destroy Terraform managed infrastructure (terraform destroy)

- Destroys all resources defined in the Terraform configuration.

### 6g. Apply formatting and style adjustments to a configuration (terraform fmt)

- Automatically formats Terraform configuration files to canonical style.

## 7. Implement and maintain state

### 7a. Describe default local backend

- **Local Backend**: By default, Terraform stores the state file locally on disk.

### 7b. Describe state locking

- **State Locking**: Prevents simultaneous updates to the state file by locking it during operations.

### 7c. Handle backend and cloud integration authentication methods

- **Authentication**: Uses various methods like AWS credentials, GCP service accounts, etc., to authenticate with backends.

### 7d. Differentiate remote state back end options

- **Remote Backends**: Options include AWS S3, Google Cloud Storage, and others for storing state files remotely.

### 7e. Manage resource drift and Terraform state

- **Resource Drift**: Regular state refresh and `terraform plan` help detect and manage changes not made through Terraform.

### 7f. Describe backend block and cloud integration in configuration

- **Backend Block**: Defines where the state is stored, e.g., in an S3 bucket.

  ```hcl
  backend "s3" {
    bucket = "my-tf-state"
    key    = "path/to/my/key"
    region = "us-east-1"
  }
  ```

  ### 7g. Understand secret management in state files

- **Secrets in State**: Sensitive data may be stored in state files, so securing state storage is critical. Use encryption and secure storage options to protect sensitive information.

## 8. Read, generate, and modify configuration

### 8a. Demonstrate use of variables and outputs

- **Variables**: Used to parameterize Terraform configurations.
- **Outputs**: Used to return values from modules or the root module.

### 8b. Describe secure secret injection best practice

- **Best Practices**: Use environment variables or secret management tools (e.g., Vault) to inject secrets securely into Terraform configurations without exposing them in code or state files.

### 8c. Understand the use of collection and structural types

- **Types**: Terraform supports collections like lists and maps, and structural types like objects and tuples, which help handle complex data structures.

### 8d. Create and differentiate resource and data configuration

- **Resources**: Used to create and manage infrastructure (e.g., `aws_instance`).
- **Data Sources**: Used to fetch existing resources or data that Terraform does not manage (e.g., `aws_ami`).

### 8e. Use resource addressing and resource parameters to connect resources together

- **Resource Addressing**: Terraform allows you to reference resources using their addresses, like `aws_instance.my_instance`. This is used to define relationships and dependencies between resources.

### 8f. Use HCL and Terraform functions to write configuration

- **HCL (HashiCorp Configuration Language)**: The syntax used for writing Terraform configuration files. It is designed to be both human-readable and machine-friendly.
- **Terraform Functions**: Terraform includes built-in functions such as `join()`, `split()`, and `lookup()` that are used to manipulate and manage data within configuration files.

### 8g. Describe built-in dependency management (order of execution based)

- **Dependency Management**: Terraform automatically manages dependencies between resources. It understands which resources need to be created or destroyed first based on their dependencies. This ensures that the infrastructure is created and modified in the correct order.

## 9. Understand HCP Terraform capabilities

### 9a. Explain how HCP Terraform helps to manage infrastructure

- **HCP Terraform**: The HashiCorp Cloud Platform (HCP) provides a managed service for Terraform that includes features like secure remote state storage, team collaboration, and governance.

### 9b. Describe how HCP Terraform enables collaboration and governance

- **Collaboration**: HCP Terraform allows multiple team members to work on the same Terraform configurations, with state management features to prevent conflicts.
- **Governance**: Includes tools for policy enforcement (such as Sentinel) to ensure compliance with organizational standards and best practices.

| Feature                       | HashiCorp Cloud Platform (HCP)                                      | Local State                                                       | External State                                                                       |
| ----------------------------- | ------------------------------------------------------------------- | ----------------------------------------------------------------- | ------------------------------------------------------------------------------------ |
| **Storage Location**          | Managed by HashiCorp in the cloud                                   | Stored on local disk of the user's machine                        | Stored in remote services (e.g., AWS S3, Azure Blob Storage)                         |
| **Access Control**            | Built-in authentication and RBAC (Role-Based Access Control)        | Access controlled by file system permissions                      | Access control managed by the external service (e.g., IAM policies for S3)           |
| **Collaboration**             | Native support for team collaboration with shared state and locking | Limited, as only the local user can modify the state              | Supports collaboration through state locking and shared access via remote storage    |
| **State Locking**             | Automatic state locking to prevent conflicts                        | No built-in locking; risk of state corruption with concurrent use | Supports state locking using a service like DynamoDB (with S3 backend)               |
| **Security**                  | Secure storage with encryption and automated backups                | Relies on local machine's security; encryption is manual          | Security features depend on the remote service (e.g., server-side encryption for S3) |
| **Backup and Recovery**       | Automatic state versioning and backups                              | Manual backups required                                           | Automatic backups and versioning can be configured (e.g., S3 versioning)             |
| **Scalability**               | Highly scalable, managed by HashiCorp                               | Limited by local machine's storage capacity and performance       | Scalable based on the chosen external storage solution                               |
| **Ease of Setup**             | Simple setup with Terraform Cloud integration                       | Very easy, no setup needed for local use                          | Requires configuration of backend and authentication                                 |
| **Cost**                      | Subscription-based pricing model for HCP                            | No cost beyond local storage                                      | Cost depends on the external storage service (e.g., AWS S3 storage fees)             |
| **Compliance and Governance** | Built-in compliance tools like Sentinel for policy enforcement      | No built-in compliance tools                                      | Compliance depends on the external service; may require custom solutions             |
