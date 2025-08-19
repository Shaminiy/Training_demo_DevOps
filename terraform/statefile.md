# Terraform State Management

Terraform's state management tracks the mapping between your configuration and real-world infrastructure in cloud.

---

## What is the Terraform State File? -> all data is present here about the resources created in cloud.
- **Purpose:** Maps Terraform resources in the configuration to actual infrastructure objects (e.g., AWS EC2 instances, GCP buckets).
- **Format:** JSON file (human-readable, but never manually edit it).
- **Location:**
  - By default, stored locally as `terraform.tfstate`.
  - For teams, use remote backends (S3, GCS, Terraform Cloud).

---

## Importance of State
1. **Tracking:** Stores resource IDs, attributes, and dependencies. Without the state file, Terraform cannot know which cloud resource to update/delete/create.
2. **Performance:** Speeds up the plan/apply process by avoiding unnecessary API calls.
3. **Collaboration:** Remote state storage (e.g., S3, GCS) allows multiple users to work on the same infrastructure without conflicts.
4. **Drift Detection:** Helps detect changes made outside of Terraform (e.g., manual changes in the cloud console).
5. **Rollback:** Allows you to revert to a previous state if needed.

---

## State File Management Strategies

1. **Local State:**
   - Default, stored in `terraform.tfstate` on your machine.
   - Suitable for single-user/personal projects (risky for teams).
2. **Remote State:**
   - For teams, store state in shared/remote backends (e.g., S3, GCS, Terraform Cloud).
   - Prevents conflicts and enables collaboration.

   **Examples:**
   - **AWS S3 Backend (with DynamoDB locking):**
     ```hcl
     terraform {
       backend "s3" {
         bucket         = "my-terraform-state-bucket"
         key            = "prod/network/terraform.tfstate"
         region         = "us-east-1"
         encrypt        = true
         dynamodb_table = "terraform-locks"  # Prevents concurrent edits
       }
     }
     ```
   - **GCP Backend:**
     ```hcl
     terraform {
       backend "gcs" {
         bucket  = "my-terraform-state-bucket"
         prefix  = "prod/network"
         # credentials = "path/to/credentials.json"
       }
     }
     ```
   - **Terraform Cloud/Enterprise Backend:**
     ```hcl
     terraform {
       backend "remote" {
         organization = "my-org"
         workspaces {
           name = "prod-network"
         }
       }
     }
     ```
3. **State Locking:** Prevents concurrent modifications (e.g., using DynamoDB with S3 backend).
4. **Versioning:** Keep versions of the state file for rollback (e.g., S3 versioning).
5. **Sensitive Data:** Use encryption and access controls for state files.

---

## Statefile Operations
Terraform provides commands to manage the state file:

- `terraform state list` — Lists all resources in the state file.
- `terraform state show <resource>` — Shows details for a specific resource.
  - Example: `terraform state show aws_instance.web_server`
- `terraform import` — Import existing infrastructure into Terraform state.
  - Example: `terraform import aws_instance.web i-1234567890abcdef0`
- `terraform state rm <resource>` — Remove a resource from the state file without destroying it.
  - Example: `terraform state rm aws_instance.web_server`
- `terraform state mv <old> <new>` — Move a resource in the state file to a new address (for refactoring).
  - Example: `terraform state mv aws_instance.web_server aws_instance.new_web_server`
- `terraform state pull` — Download the latest state file from remote backend.
  - Example: `terraform state pull > latest_state.tfstate`
- `terraform state push` — Upload a local state file to the remote backend.
  - Example: `terraform state push local_state.tfstate`
- `terraform state replace-resource <old> <new>` — Replace a resource in the state file.
  - Example: `terraform state replace-resource aws_instance.web_server aws_instance.new_web_server`
- `terraform state replace-provider <old> <new>` — Replace a provider in the state file.
  - Example: `terraform state replace-provider registry.terraform.io/hashicorp/aws registry.terraform.io/my-org/aws`

---

## Key Points
- **Never commit `terraform.tfstate` to Git**
  - Contains sensitive information and can cause conflicts in team projects.
  - Use `.gitignore` to exclude it.
- **Always use remote state for team projects**
- **Enable encryption and access controls** for remote state files to protect sensitive data (e.g., S3/GCS bucket encryption).
