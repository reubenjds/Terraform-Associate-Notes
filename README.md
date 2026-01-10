# HashiCorp Certified: Terraform Associate (004) Notes

This repository contains notes for the HashiCorp Certified: Terraform Associate (004) exam. The notes are based on the [official study guide](https://developer.hashicorp.com/terraform/tutorials/certification-004) provided by HashiCorp.

> **For the exam objectives and general information go [here](./objectives.md)**

## Table of Contents

- [HashiCorp Certified: Terraform Associate (004) Notes](#hashicorp-certified-terraform-associate-004-notes)
  - [Table of Contents](#table-of-contents)
  - [Infrastructure as Code (IaC)](#infrastructure-as-code-iac)
  - [Terraform Workflow](#terraform-workflow)
  - [Terraform Commands](#terraform-commands)
    - [Terraform Init](#terraform-init)
    - [Terraform Plan](#terraform-plan)
    - [Terraform Apply](#terraform-apply)
    - [Terraform Destroy](#terraform-destroy)
  - [Installing Terraform](#installing-terraform)
  - [Terraform Blocks](#terraform-blocks)
    - [Terraform Providers](#terraform-providers)
    - [Terraform Resources](#terraform-resources)
    - [Terraform Data](#terraform-data)
    - [Meta-Arguments and Lifecycle (4f)](#meta-arguments-and-lifecycle-4f)
    - [Custom Conditions (4g)](#custom-conditions-4g)
    - [Addressing Provider, Data, and Resource Blocks](#addressing-provider-data-and-resource-blocks)
  - [Terraform State](#terraform-state)
    - [Local State Storage](#local-state-storage)
    - [Remote State Storage](#remote-state-storage)
    - [Terraform State Commands](#terraform-state-commands)
  - [Variables](#variables)
    - [Base Types](#base-types)
    - [Sensitive Variables](#sensitive-variables)
    - [Variable Validation](#variable-validation)
    - [Complex Types](#complex-types)
    - [Ephemeral Values and Write-only Arguments (4h)](#ephemeral-values-and-write-only-arguments-4h)
  - [Outputs](#outputs)
  - [Terraform Provisioners](#terraform-provisioners)
    - [Remote Execution Provisioners](#remote-execution-provisioners)
    - [Local Exec Provisioners](#local-exec-provisioners)
  - [Terraform Modules](#terraform-modules)
    - [Accessing Terraform Modules](#accessing-terraform-modules)
    - [Using Terraform Modules](#using-terraform-modules)
    - [Declaring Modules in Code](#declaring-modules-in-code)
    - [Terraform Module Outputs](#terraform-module-outputs)
  - [Terraform Built-in Functions](#terraform-built-in-functions)
  - [Type Constraints](#type-constraints)
    - [Primitive Types](#primitive-types)
    - [Complex Types](#complex-types-1)
  - [Dynamic Blocks](#dynamic-blocks)
  - [Additional Terraform Commands](#additional-terraform-commands)
    - [Terraform fmt](#terraform-fmt)
    - [Terraform apply -replace](#terraform-apply--replace)
    - [Terraform import](#terraform-import)
    - [Terraform validate](#terraform-validate)
    - [Terraform show](#terraform-show)
    - [Terraform graph](#terraform-graph)
    - [Terraform output](#terraform-output)
    - [Terraform refresh](#terraform-refresh)
    - [Terraform console](#terraform-console)
  - [Terraform Configuration Block](#terraform-configuration-block)
  - [Terraform Workspaces](#terraform-workspaces)
  - [Debugging Terraform](#debugging-terraform)
  - [HCP Terraform and Enterprise Offerings](#hcp-terraform-and-enterprise-offerings)
    - [Hashicorp Sentinel](#hashicorp-sentinel)
    - [Use Cases for Sentinel](#use-cases-for-sentinel)
    - [HasiCorp Vault](#hasicorp-vault)
    - [Terraform Registry](#terraform-registry)
    - [HCP Terraform Workspaces](#hcp-terraform-workspaces)
    - [Terraform Cloud Workspaces](#terraform-cloud-workspaces)
  - [Benefits of HCP Terraform](#benefits-of-hcp-terraform)
    - [HCP Terraform Projects and Workspaces (8c)](#hcp-terraform-projects-and-workspaces-8c)

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

## Terraform Blocks

### Terraform Providers

- Providers are Terraform’s way of abstracting integrations with the API control layer of infrastructure vendors.
- By default, Terraform looks for providers in the Terraform Providers Registry ([Terraform Providers Registry](https://registry.terraform.io/browse/providers)).
- Providers are plugins released independently of Terraform's core software, with their own versioning.
- Custom providers can be written if needed (beyond the scope of certification).
- During initialization (via `terraform init`), Terraform finds and installs providers.
- Best practice: Providers should be pinned to a specific version to avoid breaking changes.

### Terraform Resources

- Resource blocks are fundamental building blocks in Terraform that define the infrastructure components to be managed. They create, update, and delete infrastructure resources like virtual machines, storage accounts, networking components, etc.
- **Usage**: Defined using the `resource` keyword, each block specifies the resource type and a name to reference it within the configuration.
- A resource block typically includes:
  - The `resource` keyword.
  - The type of resource (e.g., `aws_instance`, `azurerm_virtual_network`).
  - A local name to reference the resource.
  - Configuration arguments that define the resource's properties.

```hcl
resource "<PROVIDER>_<RESOURCE_TYPE>" "<NAME>" {
  # Configuration arguments
}
```

### Terraform Data

- Data blocks in Terraform are used to fetch and reference data from external sources or existing infrastructure that is not managed by the current Terraform configuration. This allows Terraform to use external information dynamically in its configurations.
- **Usage**: Data blocks are defined using the `data` keyword, followed by the type of data source and a name. The data can then be accessed and used in other parts of the Terraform configuration.
- A data block typically includes:
  - The `data` keyword.
  - The data source type (e.g., `aws_ami`, `azurerm_resource_group`).
  - A local name to reference the data.
  - Configuration arguments that specify the data to fetch.

```hcl
data "<PROVIDER>_<DATA_SOURCE_TYPE>" "<NAME>" {
  # Configuration arguments
}
```

### Meta-Arguments and Lifecycle (4f)

Meta-arguments are special arguments that can be used with any resource type to change its behavior.

- **depends_on**: Used to create explicit dependencies between resources. Use this when Terraform cannot automatically detect a relationship (hidden dependency).
- **lifecycle**: A nested block used to customize how Terraform handles resource creation, updates, and destruction.
  - **create_before_destroy**: By default, Terraform deletes a resource before creating its replacement. Setting this to `true` ensures the new resource is created first to reduce downtime.
  - **prevent_destroy**: Provides a safety measure against accidental deletion.
  - **ignore_changes**: Ignores updates to specific resource attributes.


### Custom Conditions (4g)

Custom conditions allow you to define validation logic within resources and data sources to ensure infrastructure meets specific requirements.

- **precondition**: Evaluates a condition **before** an action (like an update or creation) is taken on the resource. It ensures the environment or inputs are valid before proceeding.
- **postcondition**: Evaluates a condition **after** the resource is processed. It checks that the resulting state or attributes of the resource are what was expected.
- **Validation**: If a condition fails, Terraform stops the operation and displays a custom error message, preventing invalid infrastructure states.

### Addressing Provider, Data, and Resource Blocks

| **Block Type** | **Addressing Format**             |
| -------------- | --------------------------------- |
| **Provider**   | `provider.<provider_name>`        |
| **Data**       | `data.<data_source_type>.<name>`  |
| **Resource**   | `resource.<resource_type>.<name>` |

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

### Terraform State Commands

These commands are used to manipulate and interact with the Terraform state file directly.

| **Command**                        | **Description**                                                    | **Use Case**                                                   |
| ---------------------------------- | ------------------------------------------------------------------ | -------------------------------------------------------------- |
| `terraform state show`             | Displays detailed state information for a given resource           | Useful for inspecting current state of a specific resource     |
| `terraform state rm`               | Removes a specified resource from the state file                   | Use when a resource needs to be unmanaged by Terraform         |
| `terraform state list`             | Lists all resources currently tracked in the state                 | Helpful for viewing all managed resources                      |
| `terraform state mv`               | Moves a resource from one state to another                         | Used for refactoring or re-structuring Terraform configuration |
| `terraform state pull`             | Retrieves the state file from its remote storage location          | Use to download and view the current remote state              |
| `terraform state push`             | Uploads a local state file to the configured remote state location | Used to manually synchronize the local state with remote state |
| `terraform state replace-provider` | Replaces provider references in the state file                     | Useful when changing providers or their versions               |

## Variables

### Base Types

- **String**: Represents text.
- **Number**: Represents numerical values.
- **Bool**: Represents true or false values.

### Sensitive Variables

- Sensitive variables are used to handle sensitive information such as passwords, API keys, or any confidential data. Marking a variable as sensitive prevents its value from being displayed in the Terraform output logs, thus helping to secure sensitive data.
- **Usage**: Use the `sensitive` argument to mark a variable as sensitive.

```hcl
variable "db_password" {
  description = "The password for the database"
  type        = string
  sensitive   = true
}
```

### Variable Validation

- Variable validation ensures that input values meet specific criteria before Terraform uses them. This helps prevent configuration errors and ensures that only valid values are passed to resources.
- **Usage**: Use the `validation` block within a variable declaration to define validation rules.

```hcl
variable "port" {
  description = "The port on which the application runs"
  type        = number

  validation {
    condition     = var.port >= 1 && var.port <= 65535
    error_message = "The port number must be between 1 and 65535."
  }
}
```

### Complex Types

- **List**: A sequence of values.
- **Set**: A collection of unique values.
- **Map**: A collection of key-value pairs.
- **Object**: A complex structure of named attributes.
- **Tuple**: A sequence of values, which can be of different types.

| **Type** | **Description**                         | **Example**               |
| -------- | --------------------------------------- | ------------------------- |
| String   | Represents text                         | `"example string"`        |
| Number   | Represents numerical values             | `42`                      |
| Bool     | Represents true or false values         | `true`                    |
| List     | An ordered sequence of values           | `["item1", "item2"]`      |
| Map      | A collection of key-value pairs         | `{"key1" = "value1"}`     |
| Set      | A collection of unique values           | `set("item1", "item2")`   |
| Object   | A collection of named attributes        | `object({name=string})`   |
| Tuple    | A sequence of values of different types | `tuple([string, number])` |

### Ephemeral Values and Write-only Arguments (4h)

Terraform provides mechanisms to handle data that should not be persisted or is inherently restricted.

- **Ephemeral Values**: Resources or variables marked as ephemeral are used during the plan and apply cycle but are **never stored in the Terraform state file**. This is critical for highly sensitive temporary credentials.
- **Write-only Arguments**: Some resource arguments are "write-only," meaning you can send a value to the provider (like a password), but the provider's API does not allow reading that value back. Terraform handles these as sensitive and does not track the value in state once applied.

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

### Remote Execution Provisioners

- Remote execution provisioners are used in Terraform to run scripts or commands on the remote machine where the infrastructure is being provisioned. These provisioners allow you to perform actions such as installing software, configuring services, or running custom scripts on the remote machine.
- To use a remote execution provisioner, you specify the connection details for the remote machine, such as the SSH username, private key, and host address. The provisioner will establish an SSH connection to the remote machine and execute the specified commands or scripts.
- Remote execution provisioners are useful when you need to perform actions that cannot be achieved declaratively in Terraform, such as running configuration management tools like Ansible or executing custom scripts on the remote machine.

Example:

```hcl
resource "aws_instance" "example" {
  # resource configuration
  provisioner "remote-exec" {
    inline = [
      "sudo apt-get update",
      "sudo apt-get install -y nginx"
    ]
  }
}
```

### Local Exec Provisioners

- Local exec provisioners are used in configuration management tools like Terraform and Ansible to execute commands on the local machine during the provisioning process. They allow you to run scripts or commands on the machine where the provisioning is being performed.
- Local exec provisioners are typically used for tasks such as initializing the environment, installing dependencies, or performing local setup before deploying resources.
- To use a local exec provisioner, you specify the command or script that needs to be executed on the local machine. The provisioner will run the specified command or script in the context of the machine where the provisioning is being performed.

Example:

```hcl
resource "aws_instance" "example" {
  # resource configuration
  provisioner "local-exec" {
    command = "echo 'Hello, World!'"
  }
}
```

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

Examples of Terraform built-in functions:

| Function     | Description                                                     |
| ------------ | --------------------------------------------------------------- |
| `abs`        | Returns the absolute value of a number.                         |
| `ceil`       | Rounds a number up to the nearest whole number.                 |
| `floor`      | Rounds a number down to the nearest whole number.               |
| `max`        | Returns the maximum value from a list of numbers.               |
| `min`        | Returns the minimum value from a list of numbers.               |
| `concat`     | Concatenates multiple strings together.                         |
| `element`    | Returns the element at a specific index in a list.              |
| `length`     | Returns the length of a string or list.                         |
| `lower`      | Converts a string to lowercase.                                 |
| `upper`      | Converts a string to uppercase.                                 |
| `replace`    | Replaces occurrences of a substring in a string.                |
| `split`      | Splits a string into a list of substrings based on a delimiter. |
| `join`       | Joins a list of strings into a single string using a delimiter. |
| `format`     | Formats a string using placeholders and values.                 |
| `jsonencode` | Converts a value to its JSON representation.                    |
| `jsondecode` | Converts a JSON string to its corresponding value.              |

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

### Terraform apply -replace

> previously known as `taint` and `untaint` commands before Terraform v0.12

- **Purpose**: Replaces a resource by first destroying it and then creating a new one. This is useful when a resource needs to be recreated due to issues or changes.
- **CLI Command**: `terraform apply -replace="RESOURCE_ADDRESS"`
- **Scenarios**:
  - To force the recreation of a misbehaving or corrupted resource.
  - To ensure provisioners or other lifecycle operations run again.
  - To simulate the side effects of recreation not triggered by attribute changes.

### Terraform import

- **Purpose**: Maps existing resources to Terraform using a resource-specific identifier (ID).
- **CLI Command**: `terraform import RESOURCE_ADDRESS ID`
- **Scenarios**:
  - When needing to manage existing resources not originally created by Terraform.
  - When creation of new resources is not permitted.
  - When not in control of the initial infrastructure creation process.

### Terraform validate

- **Purpose**: Validates the syntax and internal consistency of Terraform configuration files.
- **CLI Command**: `terraform validate`
- **Scenarios**:
  - Checking for syntax errors before running `terraform plan` or `terraform apply`.
  - Validating configurations after making changes or updates.

### Terraform show

- **Purpose**: Provides a human-readable output of the Terraform state or plan file.
- **CLI Command**: `terraform show`
- **Scenarios**:
  - Reviewing the current state of resources in detail.
  - Examining the execution plan to understand changes before applying them.

### Terraform graph

- **Purpose**: Creates a visual representation of Terraform resources and their dependencies.
- **CLI Command**: `terraform graph | dot -Tpng > graph.png`
- **Scenarios**:
  - Visualizing resource dependencies and relationships.
  - Identifying potential circular dependencies or complex infrastructure layouts.

Example output:
![graph](./assets/graph.png)

### Terraform output

- **Purpose**: Retrieves the values of output variables from the Terraform state.
- **CLI Command**: `terraform output [NAME]`
- **Scenarios**:
  - Accessing the values of output variables after applying changes.
  - Using output values for further automation or integration with other systems.

### Terraform refresh

- **Purpose**: Updates the Terraform state to match the real-world resources without modifying any infrastructure.
- **CLI Command**: `terraform refresh`
- **Scenarios**:
  - Refreshing the state file to reflect changes made outside of Terraform.
  - Verifying that the state file is in sync with the actual infrastructure.

### Terraform console

- **Purpose**: Provides an interactive console for evaluating expressions and exploring Terraform functions.
- **CLI Command**: `terraform console`
- **Scenarios**:
  - Testing expressions and interpolations before using them in configurations.
  - Exploring variable values and outputs interactively.

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

| **Command**                          | **Description**                                                                                                             |
| ------------------------------------ | --------------------------------------------------------------------------------------------------------------------------- |
| `terraform workspace list`           | Lists all the workspaces in the current working directory. Displays an asterisk (\*) next to the current workspace.         |
| `terraform workspace show`           | Shows the name of the current workspace.                                                                                    |
| `terraform workspace new <name>`     | Creates a new workspace with the specified name. This also switches to the newly created workspace.                         |
| `terraform workspace select <name>`  | Switches to the specified workspace. If the workspace does not exist, it will return an error.                              |
| `terraform workspace delete <name>`  | Deletes the specified workspace. The workspace must not be currently selected, and it must be empty (no managed resources). |
| `terraform workspace select default` | Switches back to the default workspace.                                                                                     |

## Debugging Terraform

- **TF_LOG**: An environment variable to enable verbose logging. By default, logs are sent to stderr.
- **Log Levels**: TRACE, DEBUG, INFO, WARN, ERROR, with TRACE being the most verbose.
- **Persisting Logs**: Use the `TF_LOG_PATH` environment variable to save logs to a file.
- **Setting Logging in Linux**:
  - `export TF_LOG=TRACE`
  - `export TF_LOG_PATH=./terraform.log`

## HCP Terraform and Enterprise Offerings

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

### HasiCorp Vault

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

### HCP Terraform Workspaces

- **Definition**: Workspaces hosted in HCP Terraform.
- **Features**:
  - Stores old versions of state files by default.
  - Maintains a record of all execution activities.
  - All Terraform commands are executed on managed HCP Terraform VMs.

![HCP Terraform Folder](./assets/tf-cloud.png)

### Terraform Cloud Workspaces

- **Definition**: Stores alternate state files in the same working directory.
- **Feature**: Creates separate directories within the main Terraform directory.

## Benefits of HCP Terraform

- **Collaboration**: Enables a collaborative Terraform workflow.
  - Remote execution of Terraform.
  - Workspace-based organizational model.
  - Integration with version control systems (e.g., GitHub, Bitbucket).
  - Remote state management and CLI integration.
  - Private Terraform module registry.
  - Features like cost estimation and Sentinel integration.

| Feature                       | HashiCorp Cloud Platform (HCP)                                      | Local State                                                       | External State                                                                       |
| ----------------------------- | ------------------------------------------------------------------- | ----------------------------------------------------------------- | ------------------------------------------------------------------------------------ |
| **Storage Location**          | Managed by HashiCorp in the cloud                                   | Stored on local disk of the user's machine                        | Stored in remote services (e.g., AWS S3, Azure Blob Storage)                         |
| **Access Control**            | Built-in authentication and RBAC (Role-Based Access Control)        | Access controlled by file system permissions                      | Access control managed by the external service (e.g., IAM policies for S3)           |
| **Collaboration**             | Native support for team collaboration with shared state and locking | Limited, as only the local user can modify the state              | Supports collaboration through state locking and shared access via remote storage    |
| **State Locking**             | Automatic state locking to prevent conflicts                        | No built-in locking; risk of state corruption with concurrent use | Supports state locking using a service like DynamoDB (with S3 backend)               |
| **Security**                  | Secure storage with encryption and automated backups                | Relies on local machine's security; encryption is manual          | Security features depend on the remote service (e.g., server-side encryption for S3) |
| **Backup and Recovery**       | Automatic state versioning and backups                              | Manual backups required                                           | Automatic backups and versioning can be configured (e.g., S3 versioning)             |
| **Scalability**               | Highly scalable, managed by HashiCorp                               | Limited by local machine's storage capacity and performance       | Scalable based on the chosen external storage solution                               |
| **Ease of Setup**             | Simple setup with HCP Terraform integration                       | Very easy, no setup needed for local use                          | Requires configuration of backend and authentication                                 |
| **Cost**                      | Subscription-based pricing model for HCP                            | No cost beyond local storage                                      | Cost depends on the external storage service (e.g., AWS S3 storage fees)             |
| **Compliance and Governance** | Built-in compliance tools like Sentinel for policy enforcement      | No built-in compliance tools                                      | Compliance depends on the external service; may require custom solutions             |

### HCP Terraform Projects and Workspaces (8c)

HCP Terraform (formerly Terraform Cloud) organizes work differently than the open-source CLI.

- **Projects**: These act as containers for workspaces. Projects allow teams to group workspaces by environment (e.g., Production vs. Development) or by business unit.
- **Project-Level Permissions**: You can assign access rights at the project level, which automatically applies to all workspaces within that project.
- **Workspace Organization**:
    - **Workflow Types**: Workspaces can be VCS-driven (linked to GitHub/GitLab), CLI-driven (using local commands), or API-driven.
    - **Execution Mode**: Workspaces define where Terraform runs—either on HCP Terraform's managed infrastructure (Remote) or on your own hardware (Agent).