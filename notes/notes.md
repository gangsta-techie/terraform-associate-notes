# HashiCorp Certified: Terraform Associate (004) — In-Depth Notes

This file contains expanded, exam-focused, and practical notes for the HashiCorp Certified: Terraform Associate (004) certification. The content deepens each topic from the original notes, adds concrete examples, common pitfalls, and best practices you should remember for both the exam and real-world usage.

Reference: https://developer.hashicorp.com/terraform/tutorials/certification-004

> For the exam objectives and general information see `./objectives.md`

## Quick Navigation

- Infrastructure as Code (IaC)
- Core Terraform workflow and commands
- Configuration model: providers, resources, data
- State: local vs remote backends, locking, and best practices
- Variables, types, validation, and sensitive data handling
- Modules: design, inputs/outputs, versioning
- Lifecycle & meta-arguments, pre/postconditions, dynamic blocks
- Provisioners—why to avoid them and safer alternatives
- Useful CLI commands, debugging, and CI integration
- Terraform Cloud / HCP, Sentinel, Vault, Registry overview
- Exam study tips and common pitfalls

---

## Infrastructure as Code (IaC) — Deeper

- IaC is the practice of expressing infrastructure topology and configuration as code. Benefits include reproducibility, auditability, and reduced manual error.
- Declarative vs imperative: Terraform is primarily declarative — you describe desired end state and Terraform computes the actions. Imperative scripts (adhoc shell scripts, SDK calls) are procedural and describe step-by-step operations.
- Idempotence: Terraform aims for idempotent deployments—re-applying the same configuration should produce no changes once state matches the desired configuration.
- Versioning: Store configurations (and reusable modules) in version control to track changes, enable PR reviews, and rollback when needed.

## Terraform Core Workflow (practical)

1. Write the configuration (.tf files) — keep modules small and focused.
2. Initialize: `terraform init` — installs providers, configures backend, and prepares the working directory.
3. Format & validate: `terraform fmt` and `terraform validate` — maintain consistent code and catch syntax issues.
4. Plan: `terraform plan -out=tfplan` — preview changes, save the plan for auditing or later apply.
5. Apply: `terraform apply "tfplan"` or `terraform apply` — perform changes.
6. Review state and outputs: `terraform show`, `terraform output`.
7. Destroy (when necessary): `terraform destroy` — remove managed resources.

Note: Use plan files (`-out`) in CI to separate planning and applying steps and avoid drift between review and execution.

## Terraform Commands — Key details and flags

- `terraform init`:

  - `-backend-config` can override backend config at init time.
  - When changing backend, migrate state carefully (`terraform init -migrate-state`).

- `terraform plan`:

  - `-out=<file>` stores a binary plan to be applied later.
  - `-var` and `-var-file` allow injecting variables in non-interactive automation.

- `terraform apply`:

  - Prefer `terraform apply <planfile>` in CI for immutability.
  - Use `-parallelism` to control concurrent operations.

- `terraform destroy`:
  - Use `-target` sparingly for targeted destroy/create operations; this is rarely recommended for daily workflows.

## Installing Terraform — Practical tips

- Windows/macOS/Linux: download a release binary and put it on PATH, or use package managers (`choco`, `brew`, `apt`).
- For reproducible environments, use pinned versions in CI (e.g., set `TF_ENV` or install the exact binary version).

## Configuration Model — Providers, Resources, Data

### Providers

- Providers are plugins that expose resource and data source types. Always pin provider versions to avoid unexpected breaking changes, e.g.:

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

- Providers may require configuration (`region`, credentials). Keep secrets out of code — use environment variables or Vault.

### Resources

- Resources represent infrastructure objects. Resource address format: `<provider>_<type>.<name>`.
- Example creating an AWS EC2 instance (minimal):

```hcl
resource "aws_instance" "web" {
  ami           = data.aws_ami.ubuntu.id
  instance_type = var.instance_type
}
```

### Data Sources

- Data sources read information from the provider API without creating resources. Use to look up AMIs, VPC IDs, or existing infrastructure.

```hcl
data "aws_ami" "ubuntu" {
  most_recent = true
  owners      = ["099720109477"]
  filter {
    name   = "name"
    values = ["ubuntu/images/*ubuntu-*-18.04*"]
  }
}
```

## State — Concepts and Best Practices

- State file (`terraform.tfstate`) maps Terraform resources to real-world objects. It is the source of truth for Terraform's drift detection and planning.
- Never store sensitive secrets in plaintext in state if avoidable. For secrets, use Vault or the provider's secret mechanisms.

Best practices:

- Remote backend for team collaboration (S3, Azure Blob, GCS, or Terraform Cloud) with locking (DynamoDB for S3) to avoid concurrent writes.
- Enable encryption at rest and in transit for remote backends.
- Enable state versioning (e.g., S3 versioning) and automated backup retention.
- Protect state access with IAM policies; restrict `state` write permissions to CI/CD service accounts.

Example AWS S3 backend with DynamoDB locking:

```hcl
terraform {
  backend "s3" {
    bucket         = "my-terraform-state"
    key            = "project/env/terraform.tfstate"
    region         = "us-east-1"
    dynamodb_table = "my-terraform-locks"
    encrypt        = true
  }
}
```

State CLI commands you should know: `terraform state list`, `terraform state show`, `terraform state rm`, `terraform state mv`.

## Variables — Types, Validation, Sensitive

- Basic types: `string`, `number`, `bool`.
- Complex: `list(...)`, `map(...)`, `object({...})`, `tuple([...])`.
- Mark secrets `sensitive = true`; Terraform suppresses these values in output and plan.

Example with validation and sensitive:

```hcl
variable "db_password" {
  description = "DB admin password"
  type        = string
  sensitive   = true
  validation {
    condition     = length(var.db_password) >= 12
    error_message = "Password must be at least 12 characters"
  }
}
```

Important: `sensitive` prevents display — values still exist in state unless provider treats them as write-only. Avoid printing sensitive outputs.

## Outputs

- Use outputs to expose values (IP addresses, endpoints) for humans or other modules.
- Mark outputs `sensitive = true` when they contain secrets.

Example:

```hcl
output "web_ip" {
  value = aws_instance.web.public_ip
}
```

## Modules — Design and Usage

- Modules encapsulate reusable sets of resources. Keep them focused: networking, compute, database, etc.
- Version modules when publishing (use tags in the registry or Git tags for VCS sources).
- Prefer input variables for all configuration, and expose minimal outputs.

Module example (consuming a module from registry or Git):

```hcl
module "vpc" {
  source = "terraform-aws-modules/vpc/aws"
  version = "~> 3.0"
  name = "example"
  cidr = "10.0.0.0/16"
}
```

Testing modules: use Terratest or local smoke tests. Keep examples/ and a simple `example/` directory for module consumers.

## Lifecycle & Meta-Arguments

- `depends_on` for explicit ordering when implicit graph doesn't capture the dependency.
- `lifecycle` options:
  - `create_before_destroy = true` — helps avoid downtime for replacements when supported.
  - `prevent_destroy = true` — safeguards critical resources from accidental deletion.
  - `ignore_changes = ["tags"]` — useful when external systems mutate attributes you don't want Terraform to manage.

Example lifecycle block:

```hcl
resource "aws_db_instance" "db" {
  # ...
  lifecycle {
    prevent_destroy = true
    ignore_changes  = ["tags"]
  }
}
```

## Preconditions and Postconditions (custom conditions)

- `precondition` and `postcondition` allow validating resource state and inputs. Use them to fail early when assumptions are violated.

Example precondition:

```hcl
resource "aws_instance" "example" {
  # ...
  lifecycle {
    precondition {
      condition     = contains(["t3.micro", "t3.small"], var.instance_type)
      error_message = "Instance type must be t3.micro or t3.small"
    }
  }
}
```

## Dynamic Blocks

- Use when you need to generate multiple nested blocks from a list/map variable. Keep them readable and document their shape.

Example dynamic block for security group ingress rules:

```hcl
variable "ingress_rules" {
  type = list(object({ from_port = number, to_port = number, cidr = string }))
}

resource "aws_security_group" "sg" {
  name = "example"

  dynamic "ingress" {
    for_each = var.ingress_rules
    content {
      from_port   = ingress.value.from_port
      to_port     = ingress.value.to_port
      cidr_blocks = [ingress.value.cidr]
      protocol    = "tcp"
    }
  }
}
```

## Provisioners — Use with caution

- Provisioners (local-exec, remote-exec) run scripts during resource creation. They break declarative guarantees and are nondeterministic; prefer cloud-init, user data, or configuration management tools (Ansible, Chef) instead.
- If used, make them idempotent and fail-safe.

## Important CLI Commands & Utilities

- `terraform fmt` — enforce consistent formatting.
- `terraform validate` — syntax and basic validation.
- `terraform plan -out=plan` and `terraform apply plan` — separate planning and applying in CI.
- `terraform import` — bring existing resources under Terraform management; follow with `terraform state` operations and plan carefully.
- `terraform graph` — visualize dependency graph; useful to detect unexpected references.

## Debugging & Logs

- `TF_LOG=DEBUG` or `TRACE` and `TF_LOG_PATH=./tf.log` capture detailed logs for provider/plugin issues. Be careful: logs may include secrets.
- Use `terraform console` to test expressions and functions locally.

## Terraform Cloud / HCP, Sentinel, Vault, Registry — Overview

- Terraform Cloud (HCP) provides remote state, remote runs, VCS integration, private registry, and Sentinel policies.
- Sentinel is policy-as-code — write rules to enforce guardrails (e.g., deny public S3 buckets).
- Vault integrates for secrets injection and dynamic credentials; prefer it over embedding secrets in variables/state.
- Terraform Registry hosts modules and provider documentation; prefer vetted, well-documented modules.

## Security & Governance Best Practices

- Pin provider versions and module versions.
- Use remote backends with locking and encryption.
- Isolate state per environment (prod/stage) or use workspaces carefully — workspaces are not a full substitute for separate state/backends for prod vs dev.
- Avoid storing long-lived secrets in state; use Vault or provider-managed secrets.
- Use CI to run `terraform fmt`, `terraform validate`, `terraform plan` and require review of plans.

## Exam Study Tips and Common Pitfalls

- Know the commands and their typical flags (init, plan, apply, destroy, import, state commands).
- Understand state backends and locking (S3 + DynamoDB example), and when to choose remote vs local.
- Be comfortable reading and understanding HCL snippets, resource addresses, and module usage.
- Practice: build a small VPC, deploy a VM, use modules, import an existing resource, and manipulate state safely.
- Memorize lifecycle meta-arguments, pre/postconditions, and when to use `depends_on`.

## Example: Minimal project layout

```
.
├── main.tf
├── variables.tf
├── outputs.tf
├── modules/
│   └── example_module/
│       ├── main.tf
│       ├── variables.tf
│       └── outputs.tf
└── backend.tf
```

## Summary — Key takeaways

- Use remote state with locking and versioning for teams.
- Pin providers and module versions; validate code in CI.
- Keep modules small, documented, and tested.
- Avoid provisioners when possible; use configuration management or cloud-init.
- Use Sentinel & Vault (HCP) for policy and secret management when available.

---
