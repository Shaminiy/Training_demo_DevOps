# Terraform Variables

Variables allow dynamic configuration and reuse of Terraform code.

## Types
- **Primitive:** string, number, bool
- **Complex:** list, map, object, tuple, set
- **Sensitive:** Used for sensitive data like passwords, API keys

```hcl
variable "db_password" {
  type      = string
  sensitive = true
}
```

## Example: Complex Object
```hcl
variable "settings" {
  type = object({
    instance_type = string
    enable_backup = bool
    ports         = list(number)
  })
  default = {
    instance_type = "t2.micro"
    enable_backup = true
    ports         = [80, 443]
  }
}
```

## Input Variables
Used to pass values into Terraform configurations.

```hcl
variable "instance_type" {
  description = "Type of instance to create"
  type        = string
  default     = "t2.micro"
}

resource "aws_instance" "web_server" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = var.instance_type
}
```

### Setting Variables
- Command line: `terraform apply -var="instance_type=t2.small"`
- Environment: `export TF_VAR_instance_type=t2.small`
- Variable files: `terraform.tfvars` file with key-value pairs

## Output Variables
Used to extract information from Terraform configurations.

```hcl
output "instance_ip" {
  description = "Public IP of the web server"
  value       = aws_instance.web_server.public_ip
}
```

Usage: can be viwed by `terrafrom output <instance_ip>` or displayed after `terraform apply`

## Local Variables
Used to simplify complex expressions or reuse values within a configuration/module.

```hcl
locals.tf

locals {
  region = "us-east-1"
  ami_id = "ami-0c55b159cbfafe1f0"
}

main.tf

resource "aws_instance" "web_server" {
  ami           = local.ami_id
  instance_type = "t2.micro"
  tags = {
    Name = "WebServer-${local.region}"
  }
}
```

```hcl
locals.tf

locals {
        common_tags = {
          Environment = "Production"
          Owner       = "DevOps Team"
        }
      }

main.tf

resource "aws_instance" "web" {
        tags = local.common_tags
      }
```

## Variable Precedence
Terraform loads variables in this order:
1. Environment variables (TF_VAR_*)
2. terraform.tfvars
3. *.auto.tfvars
4. -var or -var-file CLI arguments
5. Default values

## Combining Local and Input Variables
```hcl
# variables.tf
variable "environment" {
  type    = string
  default = "dev"
}

# locals.tf
locals {
  bucket_name = "app-data-${var.environment}-${random_id.suffix.hex}"
}

# main.tf
resource "random_id" "suffix" {
  byte_length = 4
}
resource "aws_s3_bucket" "data" {
  bucket = local.bucket_name
}
```

## NOTE:
1. if file name: stage.tfvars then ```terraform plan -var-file="stage.tfvars"```
