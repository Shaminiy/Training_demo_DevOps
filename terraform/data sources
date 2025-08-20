# Terraform Data Sources

## What is a Data Source?
A data source in Terraform is a mechanism to fetch and reference information from outside your Terraform configuration or from resources not managed by your current Terraform state.

- **Read-only:** Data sources allow your Terraform code to be aware of and integrate with:
  1. Resources managed by other Terraform configs (e.g., different state file)
  2. Pre-existing infrastructure created manually or by other tools
  3. Information from the cloud provider

## Why Use Data Sources?
- Avoid hardcoding values—dynamically fetch values like AMI IDs, VPC IDs, etc.
- Share common configuration across multiple environments (e.g., network configs, security groups)

---

## Syntax
```hcl
data "provider_type" "localname" {
  # arguments to filter the data source
  filter {
    name   = "filter_name"
    values = ["value1", "value2"]
  }
}
```

> **Note:** Data sources are read during the plan phase, before Terraform knows what changes to apply. The fetched data can influence the plan itself. The result of a data source is not stored in the state file; it is fetched every time you run `terraform plan` or `apply` (stored in the plan file).

---

## Example: AWS Data Sources

```hcl
# Get information about the pre-existing VPC
data "aws_vpc" "main" {
  filter {
    name   = "tag:Name"
    values = ["Production-VPC"]
  }
}

# Get information about ALL subnets in that VPC
data "aws_subnets" "public" {
  filter {
    name   = "vpc-id"
    values = [data.aws_vpc.main.id]
  }
  tags = {
    Tier = "Public"
  }
}

# Get a specific subnet by its ID
data "aws_subnet" "selected" {
  id = "subnet-12345abcde"
}

# Create an application resource in the pre-existing infrastructure
resource "aws_instance" "app" {
  ami           = data.aws_ami.amazon_linux_2.id
  instance_type = "t3.large"
  subnet_id     = data.aws_subnet.selected.id
}

resource "aws_lb" "app" {
  name               = "app-lb"
  internal           = false
  load_balancer_type = "application"
  subnets            = data.aws_subnets.public.ids
}
```

---


## Data Source vs Output Variables

|                | Data Source                              | Output Variable                        |
|----------------|------------------------------------------|----------------------------------------|
| Direction      | Inbound (fetching data)                  | Outbound (exporting data)              |
| Timing         | Runs before resources are created        | Runs after resources are created        |
| Scope          | Things not managed by Terraform          | Things managed by Terraform            |
| Source         | Get info from cloud provider             | Get info from Terraform state          |
| Purpose        | Get information                          | Show/share information                 |

> **Summary:**
> - **Data Sources:** Get information INTO your Terraform project from the outside world.
> - **Output Values:** Get information OUT OF your Terraform project to the outside world.
