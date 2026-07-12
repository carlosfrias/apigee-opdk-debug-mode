# apigee-opdk-debug-mode — Reversible Bash Debug Toggle for OPDK Components

> 🔄 **Evolution note:** The automation approach from this OPDK-era role has been consolidated into the `apigee-hybrid-workspace` Ansible collection. See the successor capability in the portfolio hub: [`carlosfrias/apigee-hybrid-workspace`](https://github.com/carlosfrias/apigee-hybrid-workspace) → `bap_coe/private_cloud/` and `bap_coe/apigee_hybrid/`. The collection README explains each role group’s business value and production context.


> **An Ansible role that toggles the `-x` debug flag across all (or a single) Apigee OPDK component shell scripts — and reverses it cleanly** — enabling verbose script tracing on demand without manual edits or permanent side effects.

> [!NOTE]
> Engineering portfolio note — this project demonstrates surgical, reversible configuration management and filesystem-scoped automation in Apigee Edge Private Cloud. See the [skills assessment →](SKILLS-ASSESSMENT.md) for the expertise applied.

<!-- BEGIN Google Required Disclaimer -->

## Not Google Product Clause

This is not an officially supported Google product.
<!-- END Google Required Disclaimer -->

---

## What the role actually does

`tasks/main.yml` performs one of two mutually exclusive blocks depending on `opdk_debug_mode`:

**When `opdk_debug_mode: 'on'`**

1. **`find`** — recursively locates every file under `{{ apigee_home }}/{{ component_name | default('') }}` whose first line matches `^{{ bash_regex }}` (i.e., files starting with a bash shebang like `#!/bin/bash`).
2. **`lineinfile` with `backrefs: yes`** — appends ` -x` to the matched shebang line (`\1 -x`), enabling `set -x`–style tracing for that script.

**When `opdk_debug_mode: 'off'`**

1. **`find`** — locates files whose shebang already includes ` -x` (matches `^{{ bash_regex }} -x`).
2. **`lineinfile` with `backrefs: yes`** — strips ` -x` back off the shebang (`\1`), restoring each script to its original state.

The role is fully reversible: running it with `'on'` and then `'off'` returns every script to its prior state with no residue.

---

## Role variables (selected)

| Variable | Default | Description |
|----------|---------|-------------|
| `opdk_debug_mode` | — (required) | `'on'` enables debug tracing; `'off'` removes it |
| `apigee_home` | `/opt/apigee` | Apigee installation root |
| `component_name` | — (optional) | Target a single component; omit to sweep all components on the node |
| `bash_regex` | `#.*bash` | Regex matching bash shebang lines (first-line filter) |

---

## Usage

Enable debug tracing on all components on a node:

```yaml
- hosts: servers
  roles:
    - { role: apigee-opdk-debug-mode, opdk_debug_mode: 'on' }
```

Enable debug tracing on a single component:

```yaml
- hosts: servers
  vars:
    opdk_debug_mode: 'on'
    component_name: edge-router
  roles:
    - { role: apigee-opdk-debug-mode }
```

Disable debug tracing (restore original shebangs):

```yaml
- hosts: servers
  roles:
    - { role: apigee-opdk-debug-mode, opdk_debug_mode: 'off' }
```

---

## Provenance

Authored and maintained by **Carlos Frias** during his tenure on Apigee Edge Private Cloud. One of the operations roles in the `apigee-opdk-*` corpus — the same expertise is aggregated in the [`apigee-edge-opdk`](https://github.com/carlosfrias/apigee-edge-opdk) framework.

Contributions welcome — see [CONTRIBUTING.md](./CONTRIBUTING.md).

## License

See [LICENSE](./LICENSE).