# Terraform Workspaces & Remote Backend

## Workspaces

**Problem:**
By default, Terraform stores the state of infrastructure in a single state file. This works for a single environment, but managing multiple environments (dev, stage, prod) becomes difficult.

**Naive solution:** Copy the entire Terraform code into multiple folders for different environments. This is hard to maintain and error-prone.

**Workspaces:**
- Allow you to manage multiple distinct state files from the same Terraform configuration directory.
- Each workspace is a specific instance of your blueprint, with its own separate state file.
- The default workspace is always called `default`.

### Common Commands
- `terraform workspace list` — See which workspaces exist and which one you're currently using.
- `terraform workspace new <name>` — Create a new, empty state container.
- `terraform workspace select <name>` — Select the current workspace.
- `terraform workspace show` — Check your current workspace.

---

## Remote Backend

**Problem:**
By default, Terraform stores its state file (`terraform.tfstate`) locally. This can cause issues:
- **Concurrency:** If two people run `terraform apply` at the same time, the state file can be corrupted.
- **Loss:** If the state file is lost or deleted, you can't manage your infrastructure.
- **Security:** The state file contains sensitive data.

**Solution: Remote Backend**
- A backend determines how state is loaded and how operations like `terraform apply` are executed.
- A remote backend stores the state file in a remote, shared location that supports locking.

### Benefits
- **State Storage:** Central, durable storage (e.g., S3, Azure Storage, GCS, Terraform Cloud/Enterprise).
- **State Locking:** Prevents concurrent modifications (e.g., AWS DynamoDB). If one user is applying, others are blocked until the lock is released.

### Example: AWS S3 + DynamoDB Backend
```hcl
terraform {
  backend "s3" {
    # Storage Configuration
    bucket         = "my-company-tf-state-bucket" # Must already exist
    key            = "project-abc/network/terraform.tfstate" # Path within bucket
    region         = "us-east-1"
    encrypt        = true # Always encrypt state at rest

    # Locking Configuration
    dynamodb_table = "terraform-state-locks" # Must already exist

    # Optional: For state access from different AWS accounts
    # role_arn       = "arn:aws:iam::ACCOUNT_ID:role/TerraformStateAccess"
  }
}
```
