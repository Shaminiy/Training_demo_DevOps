# Terraform Modules & Dynamic Blocks

## Modules

Modules are like templates—a container for multiple resources used together. Every Terraform configuration has at least one module (the root module), but you can use multiple modules for better organization and reuse.

### Types of Modules
- **Local Modules:** Modules within your own codebase.
- **Public Registry Modules:** Pre-built, community-maintained modules from the [Terraform Registry](https://registry.terraform.io/).
- **Private Registry Modules:** Hosted within your organization (Terraform Cloud, Enterprise, or private repo) for sharing internal modules.
- **Git Modules:** Modules sourced from Git repositories.
- **Remote Modules:** Modules stored in HTTP URLs, S3 buckets, or GCS buckets (less common).

---

## Dynamic Block

A dynamic block lets you generate multiple nested blocks dynamically by iterating over a collection (like a list or map). It's like a `for_each` loop, but for creating inner blocks inside a resource, not the resource itself.

### Why Use Dynamic Blocks?
- Avoid repetitive code (e.g., creating many similar ingress rules or tags)

### Syntax
```hcl
resource "some_resource" "example" {
  # ... other arguments

  dynamic "<BLOCK_NAME>" {
    for_each = <COLLECTION> # List or map to iterate over
    iterator = <ITERATOR>   # (Optional) Custom iterator variable name
    content {
      # Define the *single* nested block structure
      # Use iterator.value or iterator.key to access elements
      attribute1 = iterator.value.property1
      attribute2 = iterator.value.property2
    }
  }
}
```

### Example
```hcl
variable "common_tags" {
  type = map(string)
  default = {
    Environment = "Development"
    Project     = "ProjectAlpha"
    Owner       = "DevOpsTeam"
    ManagedBy   = "Terraform"
  }
}

resource "aws_instance" "example" {
  ami           = "ami-12345678"
  instance_type = "t3.micro"

  # Dynamic tag block
  dynamic "tag" {
    for_each = var.common_tags
    content {
      key   = tag.key   # The map key (e.g., "Environment")
      value = tag.value # The map value (e.g., "Development")
    }
  }
}
```
