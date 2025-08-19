# Terraform Resources

Resources are the core components that define infrastructure objects.

- **What to create:** (e.g., a VM, a bucket, a firewall rule)
- **How to configure:** (e.g., size, region, dependencies)

## Syntax
```hcl
resource "provider_type" "logical_name" {
  # ... arguments ...
}
```
- Logical name (e.g., `web_server`) is used for references within Terraform

## Example: EC2 Instance
```hcl
resource "aws_instance" "web_server" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  tags = {
    Name = "ProductionWebServer"
  }
}
```

## Example: GCP Bucket
```hcl
resource "google_storage_bucket" "assets" {
  name     = "company-assets-bucket"
  location = "US"
}
```

### Arguments vs Attributes
- **Arguments:** Inputs you set (e.g., `ami`, `instance_type`)
- **Attributes:** Outputs Terraform provides (e.g., `public_ip`, `arn`)

## Multiple Resources
- Use `count` for identical resources
- Use `for_each` for unique configurations

```hcl
# Create 3 identical instances
resource "aws_instance" "web" {
  count         = 3
  ami           = "ami-123456"
  instance_type = "t2.micro"
  tags = {
    Name = "WebServer-${count.index}"
  }
}

# Unique configurations
resource "aws_instance" "web" {
  for_each      = toset(["dev", "stage", "prod"])
  ami           = "ami-123456"
  instance_type = "t2.micro"
  tags = {
    Name = "WebServer-${each.key}"
  }
}
```

### Dependencies
- **Implicit:** Resources can reference each other

```hcl
resource "aws_vpc" "main" {
  cidr_block = "10.0.0.0/16"
}

resource "aws_subnet" "web" {
  vpc_id     = aws_vpc.main.id  # Implicit dependency
  cidr_block = "10.0.1.0/24"
}
```

- **Explicit:** Use `depends_on` to set dependencies

```hcl
resource "aws_instance" "web" {
  depends_on = [aws_iam_role_policy.ec2_permissions]
}
```

### Lifecycle Customization
```hcl
resource "aws_instance" "web" {
  lifecycle {
    ignore_changes = [ami]  # Ignore changes to the AMI
  }
}

resource "aws_instance" "web" {
  lifecycle {
    prevent_destroy = true    # Block accidental deletion
    ignore_changes  = [tags]  # Don’t revert tag changes made outside Terraform
  }
}
```
