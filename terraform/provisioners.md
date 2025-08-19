# Terraform Provisioners

Provisioners help run setup commands or execute scripts on local or remote machines after resources have been created. This is useful for installing software, configuring services, running initialization tasks, and more.

---

## Use Cases
- Install software after creating a VM
- Run setup scripts after creating a database
- Upload files to a bucket after creation

---

## Types of Provisioners

### 1. Local Exec
- Executes commands/scripts on the local machine where Terraform runs.
- **Use cases:**
  - Notifications
  - Update local files/inventory (e.g., when a new VM is created)

### 2. Remote Exec
- Executes commands/scripts on the remote machine where the resource is created (via SSH or WinRM).
- **Use cases:**
  - Install software packages
  - Start services

### 3. File Provisioner
- Uploads/copies files from the local machine to the remote resource.
- **Use cases:**
  - Copy configuration files
  - Upload application files

---

## `null_resource`
The most powerful provisioner pattern. Use `null_resource` when you need provisioning that isn't tied to a specific resource.

```hcl
resource "null_resource" "setup" {
  triggers = {
    # Re-run when these values change
    instance_id = aws_instance.web.id
    config_hash = filemd5("config.sh")
  }

  provisioner "local-exec" {
    command = "echo 'Setting up instance ${aws_instance.web.id}'"
  }
}
```

---

## Notes
- By default, if a provisioner fails, the entire `terraform apply` fails.
- A `connection` block is required for `remote-exec` and `file` provisioners to connect to the remote machine.
- Provisioners can run before resources are ready; use explicit dependencies like `depends_on`.
- Template-based provisioning is available using the `templatefile` key.
