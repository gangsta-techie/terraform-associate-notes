# Terraform Associate — Practice Questions

This file contains 100 multiple-choice questions (MCQs) with options and answers to help revision for the HashiCorp Certified: Terraform Associate (004) exam.

---

1. What is the primary purpose of `terraform init`?

A. Apply the planned changes to the infrastructure
B. Initialize the working directory, download providers and modules
C. Validate configuration files for syntax errors
D. Destroy existing infrastructure

Answer: B/
Explanation: `terraform init` initializes the working directory, installs provider plugins and modules, and configures backends.

---

2. Which command creates an execution plan and writes it to a file?

A. `terraform apply -out=plan`
B. `terraform plan -out=plan`
C. `terraform show -out=plan`
D. `terraform validate -out=plan`

Answer: B
Explanation: `terraform plan -out=<file>` generates a plan and stores it as a binary file to apply later.

---

3. When should you use a remote backend (e.g., S3, Terraform Cloud)?

A. Only when using Windows
B. For team collaboration, locking, and centralized state
C. To avoid writing any provider blocks
D. To automatically upgrade Terraform versions

Answer: B
Explanation: Remote backends enable shared state, locking, and central management for teams.

---

4. What does `terraform apply "tfplan"` do compared to `terraform apply`?

A. Applies the plan stored in file `tfplan` (recommended in CI)
B. Shows a dry-run only
C. Destroys resources listed in `tfplan`
D. Converts `tfplan` to JSON

Answer: A
Explanation: Applying a saved plan ensures the exact reviewed plan is applied; good for CI/CD.

---

5. Which meta-argument would you use to prevent Terraform from destroying a resource by accident?

A. `prevent_destroy` in `lifecycle`
B. `ignore_changes` in `lifecycle`
C. `depends_on`
D. `create_before_destroy`

Answer: A
Explanation: `prevent_destroy = true` in a resource's `lifecycle` block blocks delete operations for that resource.

---

6. Which of these is TRUE about `variable` marked `sensitive = true`?

A. Value is encrypted in the state by Terraform automatically
B. Terraform will not display the value in CLI output or plan
C. The variable cannot be used in resources
D. The variable is stored in environment variables only

Answer: B
Explanation: `sensitive` prevents values being shown in CLI output, though they may still exist in state unless provider treats them as write-only.

---

7. Which command maps an existing cloud resource into Terraform's state?

A. `terraform import`
B. `terraform refresh`
C. `terraform state push`
D. `terraform taint`

Answer: A
Explanation: `terraform import` associates an existing resource with a resource address in state.

---

8. What is a Terraform module?

A. A single resource block only
B. A container for multiple resources used together (reusable)
C. A CLI plugin for Terraform
D. The binary distribution of Terraform

Answer: B
Explanation: Modules are reusable containers of resources; the root module is the working directory.

---

9. Which block type reads values from existing infrastructure without creating new resources?

A. `resource`
B. `module`
C. `data`
D. `provider`

Answer: C
Explanation: `data` sources fetch information from providers about existing objects.

---

10. You want Terraform to create a replacement resource before destroying the existing one. Which lifecycle argument helps with that?

A. `prevent_destroy`
B. `create_before_destroy`
C. `ignore_changes`
D. `depends_on`

Answer: B
Explanation: `create_before_destroy = true` instructs Terraform to try to create the new resource before destroying the old one.

---

11. What is the effect of `ignore_changes` in `lifecycle`?

A. Terraform permanently removes the attribute from resource configuration
B. Terraform temporarily ignores state for the resource
C. Terraform ignores future diffs to specified attributes
D. Terraform marks the resource as tainted

Answer: C
Explanation: `ignore_changes` tells Terraform not to consider changes to specified attributes when planning updates.

---

12. Which Terraform command would you use to see the current resources tracked in the state?

A. `terraform state list`
B. `terraform show` only
C. `terraform output`
D. `terraform plan`

Answer: A
Explanation: `terraform state list` shows the addresses of resources tracked in the state.

---

13. Which of the following is the recommended way to handle secrets and short-lived credentials for Terraform runs?

A. Store them directly in `.tf` files
B. Use HashiCorp Vault or provider-specific secret managers
C. Hardcode in `backend` block
D. Put them in `outputs` marked sensitive

Answer: B
Explanation: Use Vault or provider secret mechanisms to avoid secrets in code or state.

---

14. What is the use of `terraform fmt`?

A. Apply the configuration
B. Format HCL files consistently
C. Check for provider updates
D. Convert JSON to HCL

Answer: B
Explanation: `terraform fmt` formats `.tf` files to canonical layout for readability.

---

15. Which workspace is present by default in Terraform?

A. `prod`
B. `default`
C. `main`
D. `workspace-1`

Answer: B
Explanation: Terraform creates a `default` workspace initially.

---

16. When using S3 backend, which additional service is commonly used for state locking?

A. AWS Lambda
B. DynamoDB
C. SQS
D. Route53

Answer: B
Explanation: DynamoDB is used to implement state locks when S3 is the backend.

---

17. Which statement about provisioners is correct?

A. Provisioners should be the primary way to configure software on instances
B. Provisioners are deterministic and tracked fully by Terraform
C. Provisioners are last-resort; prefer cloud-init or config management tools
D. Provisioners automatically retry failed actions

Answer: C
Explanation: Provisioners are nondeterministic and should be avoided when possible.

---

18. What does `terraform import` NOT do?

A. Create a configuration file for the resource
B. Record the resource in the state
C. Allow Terraform to manage the existing resource going forward
D. Require the resource address as input

Answer: A
Explanation: `terraform import` maps existing resources into state but does not generate `.tf` configuration; you must write the config.

---

19. Which function converts a value to JSON string in Terraform?

A. `jsonparse()`
B. `jsonencode()`
C. `tojson()`
D. `encodejson()`

Answer: B
Explanation: `jsonencode()` converts values into a JSON string representation.

---

20. Which type should you use for a variable that must be a map of strings?

A. `list(string)`
B. `map(string)`
C. `object({})`
D. `tuple([string])`

Answer: B
Explanation: `map(string)` enforces a map where each value is a string.

---

21. What is the recommended practice for provider and module versioning?

A. Never pin versions; always use latest
B. Pin provider and module versions to a supported range
C. Use exact versions for providers but not modules
D. Remove `required_providers` block entirely

Answer: B
Explanation: Pinning prevents accidental breaking changes from major updates; use semantic version ranges where appropriate.

---

22. What does marking an output `sensitive = true` do?

A. Prevents the output from being written to state
B. Hides the output value from CLI output and plan
C. Encrypts the output value in the backend automatically
D. Converts output to a different HCL type

Answer: B
Explanation: `sensitive` hides displayed values; it does not necessarily remove them from state.

---

23. Which command shows a human-readable representation of the plan or state file?

A. `terraform show`
B. `terraform graph`
C. `terraform output`
D. `terraform log`

Answer: A
Explanation: `terraform show` prints a readable view of plan/state files.

---

24. Which feature is NOT a responsibility of Terraform Cloud / HCP?

A. Remote execution of Terraform runs
B. Built-in policy enforcement with Sentinel
C. Dynamic rotation of provider API keys (Vault handles secrets)
D. Remote state storage and versioning

Answer: C
Explanation: Vault or provider-specific capabilities handle dynamic secret rotation; Terraform Cloud integrates with Vault but does not itself rotate provider API keys.

---

25. Which of the following best describes dynamic blocks?

A. A way to import existing resources
B. A method to iterate over collections and generate nested blocks
C. A provider-specific debugging tool
D. A substitute for modules

Answer: B
Explanation: Dynamic blocks iterate over lists/maps to produce repeated nested configuration blocks.

---

26. Which argument lets you call a module multiple times with different inputs?

A. `count` in module block
B. `for_each` in module block
C. Both `count` and `for_each`
D. Modules cannot be instantiated multiple times

Answer: C
Explanation: Modules support `count` and `for_each` to instantiate multiple copies with different inputs.

---

27. What does `terraform validate` check?

A. Provider API connectivity
B. HCL syntax and basic internal consistency
C. Remote state integrity
D. Runtime resource creation

Answer: B
Explanation: `terraform validate` checks configuration syntax and internal consistency, not provider connectivity.

---

28. Which function returns the number of elements in a list or map?

A. `size()`
B. `length()`
C. `count()`
D. `num()`

Answer: B
Explanation: `length()` returns the number of elements in strings, lists, and maps.

---

29. What is the effect of `terraform state rm`?

A. Permanently deletes the remote resource
B. Removes a resource from the state without deleting the real resource
C. Uninstalls the provider plugin
D. Renames a resource in state

Answer: B
Explanation: `terraform state rm` removes the tracked mapping so Terraform no longer manages the resource.

---

30. Which expression creates a map from two lists `keys` and `values`?

A. `zipmap(keys, values)`
B. `merge(keys, values)`
C. `map(keys, values)`
D. `setproduct(keys, values)`

Answer: A
Explanation: `zipmap(keys, values)` pairs list elements to build a map.

---

31. Which built-in function returns the first non-null/non-error value among its arguments?

A. `coalesce()`
B. `try()`
C. `first()`
D. `compact()`

Answer: B
Explanation: `try()` returns the first expression that does not produce an error; `coalesce` handles null-ish values in some contexts.

---

32. Which command converts configuration into canonical formatting?

A. `terraform validate`
B. `terraform fmt`
C. `terraform format`
D. `terraform tidy`

Answer: B
Explanation: `terraform fmt` reformats HCL files to a standard layout.

---

33. Which of the following is TRUE about `for_each` on resources?

A. It only accepts lists
B. It requires the addresses of existing resources
C. It creates one instance per element of the collection (keys maintained for maps)
D. It cannot be used with `count`

Answer: C
Explanation: `for_each` iterates over maps or sets/lists and creates resources keyed by each element; `count` cannot be used simultaneously on same resource.

---

34. How do you pin a provider to a specific source and version?

A. In `provider` block only
B. Using `terraform { required_providers { ... } }`
C. In the `backend` block
D. You cannot pin providers

Answer: B
Explanation: `required_providers` inside the `terraform` block sets provider source and version constraints.

---

35. What is the recommended way to avoid exposing secrets in logs when using `TF_LOG`?

A. Use `TF_LOG=ERROR` only
B. Never run Terraform
C. Use `TF_LOG` but write logs to `/dev/null`
D. Avoid supplying secrets directly; use Vault or environment variables and be cautious of `TF_LOG` output

Answer: D
Explanation: `TF_LOG` can leak secrets; use secure secret management and restrict logs.

---

36. Which feature enables executing Terraform runs on a hosted service with VCS integration?

A. Terraform CLI
B. Terraform Cloud / HCP
C. Terraform Agent only
D. Terraform fmt

Answer: B
Explanation: Terraform Cloud/HCP provides VCS-driven runs and remote execution features.

---

37. What does `terraform workspace select` do?

A. Switches to another state backend entirely
B. Switches to the specified workspace (state file) within the same working directory
C. Creates a new module
D. Locks the state

Answer: B
Explanation: Workspaces are alternate state instances in the same directory; `select` switches between them.

---

38. Which statement about `terraform import` is FALSE?

A. It records the resource in state
B. It generates the `.tf` configuration for you
C. It requires you to create the matching resource block manually
D. It maps a real resource to a Terraform address

Answer: B
Explanation: `terraform import` does not create configuration files; you must write the resource definitions.

---

39. What does `terraform state mv` accomplish?

A. Moves a resource in remote provider
B. Moves resource mapping within state from one address to another
C. Merges two state files
D. Exports the state to JSON

Answer: B
Explanation: `terraform state mv` changes the address of a tracked resource in the state file.

---

40. Which function returns the intersection of two sets/lists?

A. `setunion()`
B. `setintersection()`
C. `intersect()`
D. `slice()`

Answer: B
Explanation: `setintersection()` returns common elements between collections.

---

41. Which variable type allows different attributes with specific types (e.g., `{ name = string, count = number }`)?

A. `map(string)`
B. `object({...})`
C. `tuple([...])`
D. `any`

Answer: B
Explanation: `object({...})` enforces a structured map with named attributes and types.

---

42. How can you prevent concurrent modifications to a remote S3 state?

A. Use `terraform lock` command
B. Enable DynamoDB table for state locking
C. Use `terraform workspace lock`
D. Use a single local user only

Answer: B
Explanation: DynamoDB is used with S3 backend to lock state during operations.

---

43. Which command prints the values of outputs in the current state?

A. `terraform show` only
B. `terraform outputs` (invalid)
C. `terraform output`
D. `terraform state outputs`

Answer: C
Explanation: `terraform output` retrieves outputs from the state.

---

44. Which lifecycle setting should you use to ignore provider-updated attributes (e.g., `last_modified`)?

A. `ignore_changes`
B. `prevent_destroy`
C. `create_before_destroy`
D. `replace_triggered_by`

Answer: A
Explanation: `ignore_changes` tells Terraform to ignore diffs to specified attributes.

---

45. Which CLI flag stores plan output to a file for later applying?

A. `-save` on `apply`
B. `-out` on `plan`
C. `-to-file` on `plan`
D. `-persist` on `plan`

Answer: B
Explanation: `terraform plan -out=<filename>` writes a binary plan file.

---

46. Which statement about `count` and `for_each` is correct?

A. Both can be used together on the same resource
B. `count` produces indexed instances; `for_each` can produce keyed instances
C. `for_each` always preserves order
D. `count` accepts maps only

Answer: B
Explanation: `count` creates numbered instances; `for_each` creates instances keyed by collection elements.

---

47. When Terraform shows a plan with `-(minus)` and `+(plus)` signs, what do they mean?

A. `-` indicates creation, `+` indicates deletion
B. `-` indicates deletion, `+` indicates creation
C. They show ignored changes
D. They are formatting artifacts with no meaning

Answer: B
Explanation: In plan diffs, `-` marks destroy, `+` marks create, `~` marks update.

---

48. Which provider configuration attribute can be set to use multiple provider instances (e.g., different regions)?

A. `alias`
B. `source`
C. `version`
D. `profile`

Answer: A
Explanation: `alias` allows multiple instances of the same provider with different configs.

---

49. What is `terraform graph` primarily used for?

A. Execute Terraform plans
B. Visualize the resource dependency graph
C. Validate Terraform modules
D. Convert HCL into JSON

Answer: B
Explanation: `terraform graph` helps visualize dependencies; often piped to Graphviz.

---

50. Which command would you use to apply only part of the configuration (discouraged)?

A. `terraform apply -target=`
B. `terraform apply -partial=`
C. `terraform partial-apply`
D. `terraform apply --subset=`

Answer: A
Explanation: `-target` targets specific resources, but is discouraged for regular workflows.

---

51. Which of the following is an advantage of using modules?

A. They remove the need for state files
B. Promote reuse, encapsulation, and sharing of infrastructure patterns
C. They automatically scale resources
D. They replace providers

Answer: B
Explanation: Modules package and reuse infrastructure patterns.

---

52. What does `terraform show -json` produce?

A. Pretty HCL format
B. Machine-readable JSON representation of plan/state
C. Compile-time binary
D. A list of outputs only

Answer: B
Explanation: `terraform show -json` prints a JSON document useful for automation.

---

53. Which function converts a JSON string into a Terraform value?

A. `jsonencode()`
B. `jsondecode()`
C. `fromjson()`
D. `parsejson()`

Answer: B
Explanation: `jsondecode()` parses a JSON string into its Terraform representation.

---

54. When should you use `prevent_destroy` sparingly?

A. When you want to make resources immutable and protect them from deletion
B. Always; it has no downsides
C. Only for testing environments
D. When creating temporary resources

Answer: A
Explanation: `prevent_destroy` can block legitimate infra changes; use for critical resources.

---

55. Which is a write-only argument example behavior?

A. The provider returns the value on read
B. The value is accepted on write but not returned on reads (e.g., some passwords)
C. Terraform stores the value in plain text only
D. The argument cannot be set at all

Answer: B
Explanation: Some provider fields are write-only and not readable back from the API.

---

56. What is the recommended way to share outputs between separate Terraform configurations?

A. Copy/paste the state file
B. Use remote state data sources (e.g., `terraform_remote_state` data source)
C. Use `output` in one and `input` in the other without remote backend
D. Use environment variables only

Answer: B
Explanation: `terraform_remote_state` or remote state data sources allow safe cross-configuration sharing.

---

57. Which command shows all provider plugins required by a configuration?

A. `terraform providers`
B. `terraform init -list`
C. `terraform plugins`
D. `terraform providers mirror`

Answer: A
Explanation: `terraform providers` lists provider requirements and usages in configuration.

---

58. Which feature of Terraform Cloud enforces policies as part of runs?

A. Vault
B. Sentinel
C. Workspace variables
D. Remote backend

Answer: B
Explanation: Sentinel enforces policy-as-code during Terraform Cloud runs.

---

59. What does `terraform state replace-provider` do?

A. Replaces provider plugin binary on disk
B. Rewrites state to reference a different provider source
C. Reinstalls providers with new versions
D. Converts providers to modules

Answer: B
Explanation: Useful when changing provider source addresses (e.g., namespace changes).

---

60. Which HCL construct allows you to loop and transform a collection into another?

A. Dynamic block only
B. For expression (e.g., `[for v in var.list : v * 2]`)
C. `map()` function only
D. `apply()` function

Answer: B
Explanation: For expressions are used to map/transform collections; dynamic blocks create nested blocks.

---

61. What happens if `terraform apply` detects configuration drift (resource changed outside TF)?

A. It refuses to run until drift is resolved
B. It will update state to match real-world automatically without changes
C. The plan will show the differences and propose changes to reconcile
D. It deletes the state file

Answer: C
Explanation: The plan shows differences between configuration/state and real world; you then decide to reconcile.

---

62. Which function returns the element at a specific index from a list?

A. `element(list, index)`
B. `indexof(list, index)`
C. `get(list, index)`
D. `slice(list, index)`

Answer: A
Explanation: `element()` returns an element by index; `tolist[index]` syntax also works in newer HCL.

---

63. How can you provide variables to Terraform non-interactively in automation?

A. Use `-var` and `-var-file` flags
B. Only via `terraform.tfvars` in repo
C. Terraform always prompts; cannot be non-interactive
D. Use `terraform set-var` command

Answer: A
Explanation: `-var 'name=value'` and `-var-file` pass variables in automation.

---

64. Which resource address refers to a module output named `db_endpoint` from module `db`?

A. `output.module.db.db_endpoint`
B. `module.db.db_endpoint`
C. `resource.module.db.output.db_endpoint`
D. `data.module.db.db_endpoint`

Answer: B
Explanation: Module outputs are accessed as `module.<name>.<output>`.

---

65. What is the primary difference between local and remote backends?

A. Local backends encrypt state automatically
B. Remote backends store state in a shared remote store and support locking/versioning
C. Local backends require network access
D. Remote backends cannot be used with workspaces

Answer: B
Explanation: Remote backends enable shared access, locking, and versioning; local stores state on disk.

---

66. Which command displays detailed information about a single resource in state?

A. `terraform state show <address>`
B. `terraform resource show <address>`
C. `terraform info <address>`
D. `terraform inspect <address>`

Answer: A
Explanation: `terraform state show` prints state details for an address.

---

67. What does marking a variable `nullable = false` enforce?

A. Value cannot be empty string
B. Variable must not be null; a value must be provided
C. Variable must be a list
D. Variable is sensitive

Answer: B
Explanation: `nullable = false` in type constraints prevents null values.

---

68. Which function flattens a list of lists into a single list?

A. `concat()`
B. `flatten()`
C. `merge()`
D. `flat()`

Answer: B
Explanation: `flatten()` reduces nested lists to a single-level list.

---

69. What is `terraform providers lock` used for?

A. Create a dependency lockfile to pin provider checksums (since v0.14+ `terraform providers lock` or built-in behavior)
B. Lock the state file
C. Lock workspaces
D. Prevent provider updates forever

Answer: A
Explanation: Provider dependency lock file records provider versions and checksums for reproducibility.

---

70. Which resource meta-argument allows you to reference a resource that is created by a module in a different module?

A. `depends_on` across modules
B. Use the module output in the root module and pass it as input to another module
C. `external` meta-argument
D. `remote_state` meta-argument

Answer: B
Explanation: Expose values via module outputs and pass them to other modules as inputs; direct cross-module references are not allowed.

---

71. Which function would you use to merge multiple maps into one?

A. `zipmap()`
B. `merge()`
C. `concat()`
D. `coalesce()`

Answer: B
Explanation: `merge(map1, map2, ...)` creates a merged map (later args override earlier keys).

---

72. What command do you use to list available workspaces?

A. `terraform workspaces list`
B. `terraform workspace list`
C. `terraform ws list`
D. `terraform list workspaces`

Answer: B
Explanation: `terraform workspace list` shows all workspaces in the current directory.

---

73. When writing reusable modules, which practice is recommended?

A. Hardcode region and account IDs
B. Provide sensible default variables and document inputs/outputs
C. Export all internal resources as outputs
D. Avoid versioning modules

Answer: B
Explanation: Modules should be configurable with documented inputs/outputs and reasonable defaults.

---

74. Which provider authentication method is safest for CI systems?

A. Hardcoding credentials in `.tf` files
B. Using environment variables or injected secrets from a secret manager
C. Committing credentials to `.tfvars` in repo
D. Using interactive prompt

Answer: B
Explanation: CI should get credentials from secure stores (env vars, secret managers), not stored in repo.

---

75. Which command shows who last modified the workspace in Terraform Cloud?

A. `terraform cloud history` (invalid)
B. Use the Terraform Cloud UI or API — CLI doesn't show this directly
C. `terraform whoami` (invalid)
D. `terraform workspace history`

Answer: B
Explanation: The Terraform Cloud web UI or API provides run history and actor information.

---

76. What is the purpose of `depends_on` when used inside a module block in the root module?

A. It creates implicit provider dependencies
B. It forces the module to wait for other resources to be created/destroyed first
C. It renames the module
D. `depends_on` cannot be used with module blocks

Answer: B
Explanation: `depends_on` on modules creates explicit dependencies between resources and modules.

---

77. Which Terraform command produces a graph in DOT format?

A. `terraform graph`
B. `terraform dot`
C. `terraform visualize`
D. `terraform graph -dot`

Answer: A
Explanation: `terraform graph` writes DOT output suitable for Graphviz.

---

78. Which function would you use to safely access an attribute that might not exist, avoiding an error?

A. `lookup()` or using `try()`
B. `getattr()`
C. `optional()`
D. `safe()`

Answer: A
Explanation: `lookup(map, key, default)` returns default if missing; `try()` can handle errors from expressions.

---

79. Which CLI command allows you to run an interactive expression evaluator?

A. `terraform eval`
B. `terraform console`
C. `terraform repl`
D. `terraform debug`

Answer: B
Explanation: `terraform console` provides an interactive REPL for expressions.

---

80. If a provider changes its resource schema and the provider vendor changes namespace, which command helps migrate state to the new provider?

A. `terraform state replace-provider`
B. `terraform state mv`
C. `terraform import`
D. `terraform replace-provider`

Answer: A
Explanation: `terraform state replace-provider` updates state references between provider addresses.

---

81. Which feature lets you run custom checks in Terraform Cloud that are external to Sentinel?

A. Run Triggers
B. OPA policy support
C. Run Tasks (third-party integrations)
D. Workspace variables

Answer: C
Explanation: Run Tasks allow third-party checks during runs.

---

82. Which construct allows conditional inclusion of an argument in HCL?

A. `if` statement
B. Ternary operator and `compact()`/`merge()` patterns
C. `include` block
D. `optional` argument

Answer: B
Explanation: Use ternary expressions and helper functions to conditionally build maps or lists; HCL lacks a direct `if` block.

---

83. What does `terraform refresh` do (deprecated in newer versions)?

A. Applies the plan without user confirmation
B. Reconciles state with real-world resources without changing infrastructure
C. Deletes local state
D. Validates configuration syntax

Answer: B
Explanation: `terraform refresh` updates state to match real infrastructure (behavior replaced by other commands in newer TF versions).

---

84. Which Terraform command helps detect unused providers and plugins?

A. `terraform providers` and static code inspection; `terraform init -get=false` helps detect missing providers
B. `terraform unused-providers`
C. `terraform clean-plugins`
D. `terraform providers validate`

Answer: A
Explanation: `terraform providers` lists provider usage; tooling and `init` flags help with analysis.

---

85. Which function checks if a list contains a value?

A. `contains(list, value)`
B. `in(list, value)`
C. `has(list, value)`
D. `member(list, value)`

Answer: A
Explanation: `contains()` returns true if the collection includes the element.

---

86. Which mechanism allows provider credentials to rotate dynamically during runs?

A. Using static credentials in `.tf` files
B. Integrating with Vault for short-lived credentials
C. Storing credentials in `outputs`
D. Using `terraform login`

Answer: B
Explanation: Vault can issue short-lived creds for safer automation.

---

87. Which of the following is true about `terraform fmt -check`?

A. It applies formatting changes to files
B. It returns non-zero if files are not formatted
C. It validates provider versions
D. It checks for unused variables

Answer: B
Explanation: `-check` verifies format without changing files and exits non-zero if formatting is required.

---

88. What is the recommended approach to upgrade provider versions safely?

A. Update `required_providers` then run `terraform init -upgrade` in a controlled environment and test with `plan`/`apply` in staging
B. Edit state file manually
C. Replace provider binaries without `init`
D. Use `terraform force-upgrade`

Answer: A
Explanation: Use controlled `init -upgrade` and test plans in non-production first.

---

89. Which of the following is a valid reason to use `terraform taint` (pre-v0.15) or `-replace`?

A. Force the recreation of a damaged or misconfigured resource
B. Permanently remove a resource from the state
C. Encrypt sensitive outputs
D. Convert state format

Answer: A
Explanation: `taint`/`-replace` forces recreation when necessary.

---

90. Which function returns the index of the first occurrence of a value in a list?

A. `index(list, value)`
B. `lookup(list, value)`
C. `contains(list, value)`
D. `find()`

Answer: A
Explanation: `index()` returns position of value or an error if not found; use carefully.

---

91. Which file extension can be used for variable definitions besides `.tf`?

A. `.tfvars` and `.tfvars.json`
B. `.json` only
C. `.vars` only
D. No alternatives

Answer: A
Explanation: `.tfvars` and `.tfvars.json` are accepted for variable definitions.

---

92. What is a common pitfall when using workspaces for environment separation?

A. Workspaces automatically isolate provider credentials
B. People assume workspaces isolate everything; they only provide separate state, not configuration isolation
C. Workspaces delete resources automatically
D. Workspaces are required for modules

Answer: B
Explanation: Workspaces provide separate state files but share the same configuration; separate backends/environments are often safer.

---

93. How do you reference a resource attribute from another module?

A. `module.<name>.<resource>.<attr>`
B. `module.<name>.<output>` where the module exports the attribute as an output
C. Direct cross-module resource reference is allowed
D. `output.module.<name>.<attr>`

Answer: B
Explanation: Modules should expose values via outputs; reference them as `module.name.output`.

---

94. Which Terraform feature helps you test code changes before applying to production in CI?

A. Running `terraform plan -out` in CI and requiring plan review/approval
B. Running `terraform apply` directly in production from CI
C. Skipping plans altogether
D. Using `terraform fmt` only

Answer: A
Explanation: Generate a plan and have a human/automation review before applying.

---

95. Which of these functions would you use to safely attempt multiple expressions and return the first successful one?

A. `coalesce()`
B. `try()`
C. `first()`
D. `either()`

Answer: B
Explanation: `try()` evaluates expressions and returns the first that doesn't error.

---

96. What is the purpose of `provider "local"` or similar non-cloud providers?

A. They always create cloud resources
B. They integrate with local systems or tools (e.g., local-exec-like helpers)
C. They replace AWS providers
D. They format HCL

Answer: B
Explanation: Local or third-party providers integrate with non-cloud APIs or local operations.

---

97. Which of the following is true about `terraform console` evaluation context?

A. It uses the current configuration and state, allowing evaluation of expressions using `var`, `local`, and `module` values
B. It runs in a sandbox with no access to variables
C. It only evaluates pure functions, no references allowed
D. It requires a running plan file

Answer: A
Explanation: `terraform console` loads the current configuration and state for expression evaluation.

---

98. Which practice reduces risk when importing existing resources?

A. Import directly into production
B. Create minimal resource blocks matching existing resources, import, run `plan` to reconcile, then expand config
C. Use `terraform apply` first then `import`
D. Rename resources before import

Answer: B
Explanation: Start with minimal matching config, import into state, then plan to reconcile and expand.

---

99. How can you ensure provider credentials are not accidentally committed to VCS?

A. Add credentials files to `.gitignore` and use environment variables or secret injection
B. Commit them encrypted to the repo
C. Store them in `outputs.tf`
D. Use public credentials

Answer: A
Explanation: `.gitignore` plus secure secret management and CI injection prevents leaks.

---

100. Which Terraform feature lets modules accept multiple provider configurations from the caller?

A. `providers` argument in module block to pass provider aliases
B. `provider` block inside module only
C. `required_providers` inside module only
D. Modules cannot receive providers

Answer: A
Explanation: The `providers` map in a module block allows callers to supply provider aliases for module resources.

---
