# Terraform Concepts

---

## 1. Core Concepts

| Concept    | Description                                 | Example                                      |
|------------|---------------------------------------------|----------------------------------------------|
| Resources  | Defines infrastructure components           | `resource "aws_instance" "web" { ... }`      |
| Providers  | Plugins to interact with APIs/cloud services| `provider "aws" { region = "us-east-1" }`    |
| Variables  | Input parameters for configuration          | `variable "instance_type" { default = "t2.micro" }` |
| Outputs    | Exported values for other configs/CLI       | `output "ip" { value = aws_instance.web.public_ip }` |
| State      | Tracks resource relationships in state file | Managed via backends (local/S3/GCS)          |

---

## 2. Configuration Organization

| Concept        | Description                        | When to Use                                 |
|----------------|------------------------------------|---------------------------------------------|
| Modules        | Reusable config packages           | `module "vpc" { source = "./modules/vpc" }` |
| Workspaces     | Isolated state environments        | `terraform workspace new dev`               |
| Remote State   | Shared state storage for teams     | Backend config in S3/GCS                    |
| Data Sources   | Fetch external data for config     | `data "aws_ami" "ubuntu" { ... }`           |

---

## 3. Execution Concepts

| Concept   | Description                        | CLI Command                      |
|-----------|------------------------------------|----------------------------------|
| Plan      | Preview changes before applying    | `terraform plan`                 |
| Apply     | Execute changes to infrastructure  | `terraform apply`                |
| Destroy   | Remove managed infrastructure      | `terraform destroy`              |
| Refresh   | Sync state with real infrastructure| `terraform refresh`              |
| Taint     | Force recreation of resources      | `terraform taint aws_instance.web`|

---

## 4. Advanced Concepts

| Concept         | Description                          | Example                                 |
|-----------------|--------------------------------------|-----------------------------------------|
| Provisioners    | Execute scripts on resources         | `provisioner "file" { ... }`            |
| Dynamic Blocks  | Generate repeating nested configs    | `dynamic "ingress" { for_each = var.rules }` |
| Meta-Arguments  | Special resource behavior arguments  | `count`, `for_each`, `depends_on`       |
| Workspace Vars  | Env-specific variable overrides      | `terraform.tfvars` per workspace        |
| Sentinel/OPA    | Policy-as-code enforcement           | Enterprise feature                      |

---

## 5. State Management

| Concept         | Description                        | Importance/Usage                        |
|-----------------|------------------------------------|-----------------------------------------|
| State Locking   | Prevents concurrent operations     | Critical for teams                      |
| State Inspection| View current state                 | `terraform show`                        |
| State Import    | Adopt existing infrastructure      | `terraform import aws_instance.web i-123456` |
| State Migration | Move resources between states      | For refactoring                         |

---

## 6. Dependency Graph

Terraform automatically builds a dependency graph to:

- Determine execution order
- Parallelize non-dependent operations
- Handle implicit dependencies (via references)

---

## 7. Security Concepts

| Concept            | Description                        | Implementation                |
|--------------------|------------------------------------|-------------------------------|
| Sensitive Vars     | Marks outputs/vars as secret       | `sensitive = true`            |
| Backend Security   | Encrypted state storage            | S3+KMS, GCS encryption        |
| IAM Integration    | Least-privilege provider access    | AWS IAM roles/policies        |

---

## 8. Testing & Validation

| Concept         | Description                        | Tools/Methods                 |
|-----------------|------------------------------------|-------------------------------|
| Syntax Validation| Check HCL formatting              | `terraform validate`          |
| Plan Review     | Manual inspection of changes       | CI pipelines                  |
| Unit Testing    | Verify module behavior             | Terratest, Go tests           |
| Cost Estimation | Predict infrastructure costs       | infracost, Terraform Cloud    |

---

## 9. Collaboration Features

| Concept           | Description                        | Solution                      |
|-------------------|------------------------------------|-------------------------------|
| Remote Operations | Centralized execution              | Terraform Cloud/Enterprise    |
| Policy Enforcement| Governance rules                   | Sentinel, OPA                 |
| Run Triggers      | Automate based on changes          | Linked workspaces             |

---

## 10. Terraform Ecosystem

| Component   | Purpose                                 |
|-------------|-----------------------------------------|
| Registry    | Public/private module repository        |
| Cloud       | Managed Terraform collaboration         |
| CDKTF       | Terraform with programming languages    |
| Terragrunt  | Terraform workflow automation           |
