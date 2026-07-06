# Skills Assessment — apigee-opdk-debug-mode

> **Skill domain:** Apigee OPDK operational debugging — reversible, filesystem-scoped bash debug toggling for distributed system troubleshooting. Part of the broader Apigee platform-operations portfolio; see the [`bap_coe` portfolio hub →](https://github.com/carlosfrias/apigee-hybrid-workspace/blob/main/SKILLS-ASSESSMENT.md) for the full corpus.

---

## Why this role is notable

- **Reversible by design.** Running with `opdk_debug_mode: 'on'` then `'off'` returns every script to its prior state with no residue — no manual cleanup, no permanent side effects.
- **Filesystem-scoped, not process-scoped.** Instead of `set -x` in a running process, this role finds and modifies the bash shebang lines of OPDK component scripts. When the component restarts, it inherits the debug flag from the shebang.
- **Component-selective.** Set `component_name` to target a single component, or omit it to sweep all components on the node. Surgical or broad — the operator chooses.

---

## Expertise demonstrated

> Ansible is the medium. The engineering evidence lives in the [project README →](README.md). What follows is the skills assessment for the business reader.

- **Reversible configuration management** — the role is a toggle, not a one-way change. `on` adds ` -x` to shebangs; `off` strips it back. No residue, no manual cleanup.
- **Filesystem-scoped automation** — `find` locates bash scripts, `lineinfile` with `backrefs` modifies the shebang. The automation is scoped to the filesystem, not the process — it works even when the process isn't running.
- **Component-selective targeting** — `component_name` variable lets the operator target a single component or sweep the entire node. Surgical or broad, the operator chooses.
- **Operational debugging discipline** — debug mode is a controlled, reversible operation, not "log in and edit scripts." The role makes debugging a first-class operational procedure.

---

## How this shows the expertise

The expertise is not "editing bash shebangs" — it is **making debug mode a first-class, reversible operational procedure**. In a distributed system with 11+ component types across dozens of nodes, turning on debug tracing is a high-stakes operation. This role makes it safe: it's idempotent, reversible, component-selective, and leaves no residue. That is operational discipline applied to debugging.

---

## Related expertise

| Skill | Repository | Assessment |
|-------|-----------|-----------|
| Apigee Hybrid / K8s automation (collection) | [`apigee-hybrid-workspace`](https://github.com/carlosfrias/apigee-hybrid-workspace) | [SKILLS-ASSESSMENT.md →](https://github.com/carlosfrias/apigee-hybrid-workspace/blob/main/SKILLS-ASSESSMENT.md) ✅ portfolio hub |
| Rolling upgrade / DR / traffic fencing | [`apigee-opdk-playbook-maintenance-opdk-upgrade`](https://github.com/carlosfrias/apigee-opdk-playbook-maintenance-opdk-upgrade) | [SKILLS-ASSESSMENT.md →](https://github.com/carlosfrias/apigee-opdk-playbook-maintenance-opdk-upgrade/blob/main/SKILLS-ASSESSMENT.md) ✅ |

---

## Provenance

Authored and maintained by **Carlos Frias** during his tenure on Apigee Edge Private Cloud. This skills assessment is the companion to the engineering [README →](README.md).

## License

See [LICENSE](./LICENSE).