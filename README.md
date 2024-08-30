# HashiCorp Certified: Terraform Associate (003) Notes

This repository contains notes for the HashiCorp Certified: Terraform Associate (003) exam. The notes are based on the [official study guide](https://learn.hashicorp.com/tutorials/terraform/associate-study) provided by HashiCorp.

> **For the exam objectives and general information go [here](./objectives.md)**

## Table of Contents

1. [Infrastructure as Code (IaC)](#infrastructure-as-code-iac)
2. [Terraform Workflow](#terraform-workflow)
3. [Terraform Commands](#terraform-commands)
   1. [Terraform Init](#terraform-init)
   2. [Terraform Plan](#terraform-plan)
   3. [Terraform Apply](#terraform-apply)
   4. [Terraform Destroy](#terraform-destroy)
4. [Installing Terraform](#installing-terraform)
5. [Terraform Providers](#terraform-providers)
6. [Terraform State](#terraform-state)
   1. [Local State Storage](#local-state-storage)
   2. [Remote State Storage](#remote-state-storage)
7. [Variables](#variables)
   1. [Base Types](#base-types)
   2. [Complex Types](#complex-types)
8. [Outputs](#outputs)
9. [Terraform Provisioners](#terraform-provisioners)
10. [Terraform Modules](#terraform-modules)
11. [Terraform Built-in Functions](#terraform-built-in-functions)
12. [Type Constraints - Terraform Variables](#type-constraints-terraform-variables)
13. [Dynamic Blocks](#dynamic-blocks)
14. [Terraform CLI Utilities](#terraform-cli-utilities)
    1. [Terraform fmt](#terraform-fmt)
    2. [Terraform taint](#terraform-taint)
    3. [Terraform import](#terraform-import)
15. [Terraform Configuration Block](#terraform-configuration-block)
16. [Terraform Workspaces](#terraform-workspaces)
17. [Debugging Terraform](#debugging-terraform)
18. [Terraform Cloud and Enterprise Offerings](#terraform-cloud-and-enterprise-offerings)
    1. [Hashicorp Sentinel](#hashicorp-sentinel)
    2. [Terraform Vault](#terraform-vault)
    3. [Terraform Registry](#terraform-registry)
    4. [Terraform Cloud Workspaces](#terraform-cloud-workspaces)
    5. [Terraform OSS Workspaces](#terraform-oss-workspaces)
    6. [Benefits of Terraform Cloud](#benefits-of-terraform-cloud)
19. [Benefits of Terraform Cloud](#benefits-of-terraform-cloud)

---

## Infrastructure as Code (IaC)

- **No More Clicks**: Write down what you want to deploy (VMs, disks, apps, etc.) as human-readable code.
- **Enables DevOps**: Codification of deployment means it can be tracked in version control, enabling better visibility and collaboration across teams.
- **Declare Your Infrastructure**: Infrastructure is typically written declaratively via code but can be procedural (imperative) too.
- **Speed, Cost, and Reduced Risk**: Less human intervention during deployment reduces chances of security flaws, unnecessary resources, and saves time.

## Terraform Workflow

1. **Write Your Terraform Code**

   - Start by creating a GitHub repo as a common best practice.

2. **Review**

   - Continually add and review changes to the code in your project.

3. **Deploy**
   - After one last review/plan, you'll be ready to provision real infrastructure.

## Terraform Commands

### Terraform Init

- Initializes the working directory that contains your Terraform code.
- Downloads ancillary components such as modules and plugins.
- Sets up the backend for storing the Terraform state file.

### Terraform Plan

- Reads the code and creates a "plan" of execution or deployment.
- This command does not deploy anything; it is considered a read-only command.
- Allows users to review the action plan before executing any changes.

### Terraform Apply

- Deploys the instructions and statements defined in the code.
- Updates the deployment state tracking mechanism, known as the "state file."

### Terraform Destroy

- Looks at the recorded state file and destroys all resources created by the code.
- This is a non-reversible command, so use it with caution. Backups are recommended.

## Installing Terraform

1. **Method 1: Download, Unzip, Use**

   - Download the zipped binary from the HashiCorp website.
   - Unzip the Terraform binary.
   - Place it in your system’s `$PATH` as a best practice.

2. **Method 2: Set Up Terraform Repository on Linux**
   - Set up a HashiCorp Terraform repository on Linux (Debian, RHEL, Amazon Linux).
   - Use a package manager to install Terraform.
   - The package manager installs and sets it up for immediate use.

## Terraform Providers

- Providers are Terraform’s way of abstracting integrations with the API control layer of infrastructure vendors.
- By default, Terraform looks for providers in the Terraform Providers Registry ([Terraform Providers Registry](https://registry.terraform.io/browse/providers)).
- Providers are plugins released independently of Terraform's core software, with their own versioning.
- Custom providers can be written if needed (beyond the scope of certification).
- During initialization (via `terraform init`), Terraform finds and installs providers.
- Best practice: Providers should be pinned to a specific version to avoid breaking changes.

## Terraform State

- **Resource Tracking**: A mechanism for Terraform to keep tabs on deployed resources.
- Stored in flat files, typically named `terraform.tfstate`.
- Helps Terraform calculate deployment deltas and create new deployment plans.
- It's crucial not to lose your Terraform state file.

### Local State Storage

- Saves the Terraform state file locally on your system.
- This is Terraform's default behavior.

### Remote State Storage

- Saves the state file to a remote data source (e.g., AWS S3, Google Storage).
- Allows sharing the state file between distributed teams.
- Remote state is accessible to multiple teams for collaboration.
- Enables state locking to prevent coinciding parallel executions.
- Supports sharing "output" values with other Terraform configurations or code.

## Variables

### Base Types

- **String**: Represents text.
- **Number**: Represents numerical values.
- **Bool**: Represents true or false values.

### Complex Types

- **List**: A sequence of values.
- **Set**: A collection of unique values.
- **Map**: A collection of key-value pairs.
- **Object**: A complex structure of named attributes.
- **Tuple**: A sequence of values, which can be of different types.

## Outputs

- Output variables display values in the shell after running `terraform apply`.
- Output values act like return values you want to track after a successful Terraform deployment.

## Terraform Provisioners

- Provisioners bootstrap custom scripts, commands, or actions during deployment.
- They can run locally (on the system where Terraform is executed) or remotely on deployed resources.
- Each resource can have its own provisioner, defining the connection method.
- **Note**: HashiCorp recommends using provisioners as a last resort. Use inherent mechanisms for custom tasks where possible.
- Terraform cannot track changes made by provisioners, as they can take independent actions.
- Provisioners are ideal for invoking actions not covered by Terraform's declarative model.
- A non-zero return code from a provisioner marks the resource as tainted.

## Terraform Modules

- A module is a container for multiple resources used together.
- Every Terraform configuration has at least one module, known as the root module, consisting of code files in the main working directory.

### Accessing Terraform Modules

- Modules can be downloaded or referenced from:
  - Terraform public registry
  - A private registry
  - Your local system
- Modules are referenced using a `module` block.
- Additional parameters for module configuration:
  - `count`
  - `for_each`
  - `providers`
  - `depends_on`

### Using Terraform Modules

- Modules can take input and provide output to integrate with the main code.

### Declaring Modules in Code

- Module inputs are named parameters passed inside the module block and used as variables inside the module code.

### Terraform Module Outputs

- Outputs declared inside module code can feed back into the root module or main code.
- Output convention: `module.<name-of-module>.<name-of-output>`

## Terraform Built-in Functions

- Terraform comes with pre-packaged functions to help transform and combine values.
- User-defined functions are not allowed; only built-in ones can be used.
- General syntax: `function_name(arg1, arg2, …)`
- Built-in functions help make Terraform code dynamic and flexible.

## Type Constraints

### Primitive Types

- **Number**
- **String**
- **Bool**

### Complex Types

- **List**
- **Tuple**
- **Map**
- **Object**
- **Collection**: These allow multiple values of one primitive type to be grouped together.

  - Constructors for collections include:
    - `list(type)`: A list of values of a specific type.
    - `map(type)`: A map of keys to values, all of a specific type.
    - `set(type)`: A set of unique values of a specific type.

- **Structural Types**: These allow multiple values of different primitive types to be grouped together.

  - Constructors for structural collections include:
    - `object(type)`: An object with named attributes, each having a type.
    - `tuple(type)`: A tuple that can have a fixed number of elements, each with a different type.

- **Any Constraint**: This is a placeholder for a primitive type that has yet to be decided. The actual type is determined at runtime.

## Dynamic Blocks

- **Purpose**: Dynamic blocks construct repeatable nested configuration blocks inside Terraform resources.
- **Supported Block Types**: Dynamic blocks are supported within the following block types:

  - `resource`
  - `data`
  - `provider`
  - `provisioner`

- **Usage**: Dynamic blocks make your code cleaner by reducing redundancy. They act like a for loop, outputting a nested block for each element in a complex variable type.

- **Caution**: Overuse of dynamic blocks can make code hard to read and maintain. Use them to build a cleaner user interface when writing reusable modules.

## Additional Terraform Commands

### Terraform fmt

- **Purpose**: Formats Terraform code for readability and ensures consistent styling across the codebase.
- **Usage**: Safe to run at any time to maintain code quality.
- **CLI Command**: `terraform fmt`
- **Scenarios**:
  - Before pushing code to version control.
  - After upgrading Terraform or its modules.
  - Any time code changes are made.

### Terraform taint

- **Purpose**: Taints a resource, forcing it to be destroyed and recreated. This modifies the state file, leading to a recreation workflow during the next `apply`.
- **CLI Command**: `terraform taint RESOURCE_ADDRESS`
- **Scenarios**:
  - To ensure provisioners run again.
  - To replace misbehaving resources.
  - To mimic side effects of recreation not modeled by resource attributes.

### Terraform import

- **Purpose**: Maps existing resources to Terraform using a resource-specific identifier (ID).
- **CLI Command**: `terraform import RESOURCE_ADDRESS ID`
- **Scenarios**:
  - When needing to manage existing resources not originally created by Terraform.
  - When creation of new resources is not permitted.
  - When not in control of the initial infrastructure creation process.

## Terraform Configuration Block

- **Purpose**: A special block for controlling Terraform’s own behavior. This block accepts only constant values.
- **Examples**:
  - Configuring backends for state file storage.
  - Specifying a required Terraform version.
  - Specifying required provider versions.
  - Enabling and testing Terraform experimental features.
  - Passing metadata to providers.

## Terraform Workspaces

- **Definition**: Terraform workspaces are alternate state files within the same working directory.
- **Default Workspace**: Terraform starts with a single workspace named `default`, which cannot be deleted.
- **Scenarios**:
  - Testing changes using a parallel, distinct copy of infrastructure.
  - Workspaces can be modeled against branches in version control systems like Git.
- **Collaboration**: Workspaces facilitate resource sharing and team collaboration.
- **Access**: The current workspace name is available via the `${terraform.workspace}` variable.

## Debugging Terraform

- **TF_LOG**: An environment variable to enable verbose logging. By default, logs are sent to stderr.
- **Log Levels**: TRACE, DEBUG, INFO, WARN, ERROR, with TRACE being the most verbose.
- **Persisting Logs**: Use the `TF_LOG_PATH` environment variable to save logs to a file.
- **Setting Logging in Linux**:
  - `export TF_LOG=TRACE`
  - `export TF_LOG_PATH=./terraform.log`

## Terraform Cloud and Enterprise Offerings

### Hashicorp Sentinel

- **Definition**: A Policy-as-Code tool integrated with Terraform to enforce compliance and best practices.
- **Language**: Uses the Sentinel policy language, which is designed to be understandable by non-programmers.
- **Benefits**:
  - **Sandboxing**: Acts as a guardrail for automation.
  - **Codification**: Makes policies easier to understand and collaborate on.
  - **Version Control**: Allows policies to be version-controlled.
  - **Testing and Automation**: Supports automated policy enforcement.

### Use Cases for Sentinel

- Enforcing CIS (Center for Internet Security) standards across AWS accounts.
- Restricting instance types to only allow `t3.micro`.
- Ensuring security groups do not permit traffic on port 22.

### Terraform Vault

- **Definition**: A secrets management tool that dynamically provisions and rotates credentials.
- **Security**: Encrypts sensitive data both in transit and at rest, providing fine-grained access control using ACLs.
- **Benefits**:
  - Reduces the need for developers to manage long-lived credentials.
  - Injects secrets into Terraform deployments at runtime.
  - Offers fine-grained ACLs for accessing temporary credentials.

### Terraform Registry

- **Definition**: A repository for publicly available Terraform providers and modules.
- **Features**:
  - Publish and share custom modules.
  - Collaborate with contributors on provider and module development.
  - Directly reference modules in Terraform code.

### Terraform Cloud Workspaces

- **Definition**: Workspaces hosted in Terraform Cloud.
- **Features**:
  - Stores old versions of state files by default.
  - Maintains a record of all execution activities.
  - All Terraform commands are executed on managed Terraform Cloud VMs.

### Terraform OSS Workspaces

- **Definition**: Stores alternate state files in the same working directory.
- **Feature**: Creates separate directories within the main Terraform directory.

## Benefits of Terraform Cloud

- **Collaboration**: Enables a collaborative Terraform workflow.
  - Remote execution of Terraform.
  - Workspace-based organizational model.
  - Integration with version control systems (e.g., GitHub, Bitbucket).
  - Remote state management and CLI integration.
  - Private Terraform module registry.
  - Features like cost estimation and Sentinel integration.
