# Terraform Version Management with tfenv

## Installing Terraform
- By default, installing Terraform gives you the latest version.
- For managing multiple versions, use a tool like **tfenv**.

---

## What is tfenv?
**tfenv** is a Terraform version manager. It allows you to install and switch between multiple versions of Terraform easily.

### Why tfenv?
- Install multiple versions of Terraform side-by-side.
- Automatically switch versions based on the project you're working on.

---

## Scenario
- **Project A** (work): Uses Terraform v1.2.0
- **Project B** (personal): Uses Terraform v1.6.0

If you forget to switch versions, running `terraform plan` with the wrong version can cause errors (e.g., state file incompatibility). tfenv helps avoid this problem.

---

## Features of tfenv

### 1. Project-Specific Versions
- Create a `.terraform-version` file in your project directory.
- Ensures everyone (including CI/CD) uses the exact same version.
- Eliminates "works on my machine" problems.

### 2. Install and Use Multiple Versions
```sh
tfenv install 1.5.0
tfenv install 1.6.0
tfenv install latest
tfenv list
tfenv use <version>   # Switch to a specific version
```
