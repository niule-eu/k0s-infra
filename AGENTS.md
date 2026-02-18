# KB for agents

## PKL Language Basics

### Comments
- **Single-line comments** use `//` (not `#`)
- **Multi-line comments** use `/* */`
- **Example:**
  ```pkl
  // This is a single-line comment
  /* This is a
     multi-line comment */
  ```

### Common Syntax
- **Variables:** `hidden variableName = "value"`
- **Objects:** `new { key = "value" }`
- **Lists:** `new Listing { "item1", "item2" }`
- **String interpolation:** `"Hello ${name}!"`

## How to

### Rendering PKL to YAML Manifests

**Recommended Workflow:** The PKL-based infrastructure uses a two-level rendering system that works best when rendering kustomization files.

1. **Individual Controller PKL Files** (e.g., `argo-workflows.pkl`)
   - Define Kubernetes resources using Pkl language
   - Specify their output files (e.g., `argo-workflows.yaml`)
   - Contain the actual resource definitions and configurations
   - **Optimization Tip:** For resource-constrained environments, disable unnecessary features in Helm values

2. **Kustomization PKL Files** (e.g., `kustomization.pkl`)
   - Automatically discover all other PKL files in the directory
   - Aggregate all output files from individual controllers
   - Generate the final `kustomization.yaml` that references all manifests

**Standard Rendering (Recommended):**
``` bash
# For complete infrastructure rendering, use kustomization.pkl
# This automatically includes all controller PKL files in the directory
# IMPORTANT: Use absolute path to the directory containing PklProject

hlcli render-pkl -p /absolute/path/to/k0s-infra infrastructure/controllers/kustomization.pkl -f
```

**Manual Rendering (For Debugging/Testing):**
``` bash
# You CAN manually render individual controller files for debugging
# This is useful for testing a single controller before full integration
# IMPORTANT: Use absolute path to the directory containing PklProject

hlcli render-pkl -p /absolute/path/to/k0s-infra infrastructure/controllers/argo-workflows.pkl -f
```

**What happens during rendering:**
- Kustomization.pkl imports all `*.pkl` files in the directory
- Each controller generates its specified YAML output files
- The kustomization creates a final `kustomization.yaml` referencing all YAML files
- All files are written to the filesystem

**Important Notes:**
- **Absolute Path Required**: The `-p` flag MUST point to the absolute path containing the `PklProject` file
- **Relative Paths Fail**: Using relative paths like `.` or `./` will cause rendering to fail
- **PklProject Dependency**: The tool needs access to the project's dependencies defined in `PklProject`

**Best Practices:**
- ✅ **Prefer** rendering kustomization.pkl for complete infrastructure
- ✅ **Use** manual rendering for debugging individual controllers
- ✅ New controllers are automatically discovered - no kustomization.pkl changes needed
- ✅ Manual rendering is supported but may require additional steps for full integration
- ✅ Always use absolute paths with the `-p` flag

## Commit Message Style Guide

**Objective:** Write concise, functional commit messages that explain purpose, not implementation details.

**Guidelines:**
- **Focus on "why" and "what it achieves"** rather than "what was changed"
- **Be objective and technical** - avoid subjective language
- **Prioritize function and purpose** over specific file modifications
- **Keep it concise** - one line for simple changes, short paragraph for complex ones
- **Use present tense** for consistency

**Good Examples:**
- "Limit Argo Workflows parallelism to prevent resource exhaustion"
- "Optimize resource constraints for 8GB node environment"
- "Enable workflow cleanup to reduce storage usage"
- "Configure Argo Workflows for constrained single-node deployment"

**Avoid:**
- "Updated argo-workflows.pkl and argo-workflows.yaml files"
- "Changed parallelism from 1 to 2 in the configuration"
- "Added controller.parallelism setting to Helm values"
- "Modified the PKL file to include new settings"

**Rationale:** Commit messages should help future maintainers understand the purpose and reasoning behind changes, not just what files were modified. This approach creates a more useful git history for debugging and maintenance.
