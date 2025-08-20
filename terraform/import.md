# Terraform Import Concept

Terraform's import feature helps you bring resources that were created manually or outside of Terraform under Terraform management, without recreating them.

In simple, This helps to import resources which are created manually or not part of the current statefile.
for example: you manually created a VM in GCP, now you want to automate using terraform , then import option helps to get into terraform config.

---

## Why Import?
- Import resources that already exist (e.g., a VM created manually in GCP or AWS) so Terraform can manage them.
- our goal is to ensure that `terraform plan` and `terraform apply` do not try to create resources that already exist.

---

## Syntax
```sh
terraform import <resource_type>.<resource_name> <resource_ID>
```

---

## Example Usage

### Case 1: Using an Import Block
```hcl
import {
  to = aws_instance.example
  id = "i-abcd1234"
}

resource "aws_instance" "example" {
  name = "hashi"
  # (other resource arguments...)
}
```

### Case 2: Manual Import
- Create the resource block in your configuration first.
- Run the import command:
  ```sh
  terraform import aws_instance.example i-abcd1234
  ```
- Then run `terraform plan` and `terraform apply`.

### Case 3: Generate Configuration
- You can generate the configuration file before importing:
  ```sh
  terraform plan -generate-config-out="generated_resources.tf"
  ```

---

## Summary
To import a resource using blocks, you must:
1. Define an import block for the resource(s).
2. Add a corresponding resource block to your configuration, or generate configuration for that resource.
3. Run `terraform plan` to review how Terraform will import the resource(s).
4. Apply the configuration to import the resources and update your Terraform state.
