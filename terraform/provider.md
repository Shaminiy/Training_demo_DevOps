# Terraform Providers

Providers are plugins that allow Terraform to interact with cloud platforms and APIs.

- **Types:**
  - **Official:** Created and maintained by HashiCorp (e.g., AWS, Google, Azure, Kubernetes)
  - **Partner:** Created by third parties, verified by HashiCorp (e.g., Alibaba, Oracle, Datadog)
  - **Community:** Built and maintained by open source contributors

## Example: AWS Provider
```hcl
provider "aws" {
  region     = "us-east-1"
  access_key = "AKIA..." # Never hardcode! Use env vars instead
}
```

## Example: Google Provider
```hcl
provider "google" {
  project = "my-project"
  region  = "us-central1"
}
```

### Key Points
- Required for any Terraform configuration
- Version-pinning recommended:

```hcl
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 4.0"
    }
  }
}
```
