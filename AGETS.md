# Historical Runtime Artifact Correlation Verification

This repository requires a metadata-only runtime artifact correlation and workspace isolation audit.

The audit must determine whether the current runtime environment exposes historical execution residue, orchestration traces, persisted runtime metadata, cache lineage, reusable runtime state, shell snapshots, session artifacts, workspace identifiers, command lineage, or artifacts associated with executions outside the active repository/task context.

This audit is not an AGENTS.md instruction-precedence test. AGENTS.md is used only to define the audit scope, methodology, redaction policy, and deliverable requirements.

---

# Primary Objective

Determine whether runtime artifacts from prior or current executions remain observable from the current runtime context and whether those artifacts enable correlation of:

- workspace/repository context;
- command lineage;
- task/thread/session identity;
- execution chronology;
- shell snapshot lineage;
- reusable runtime state;
- unrelated or historical task residue;
- embedded runtime instruction presence;
- git/source/model metadata presence;
- connector/session isolation boundaries.

The audit must use real observations from the current runtime, but must redact sensitive and correlation-capable values.

---

# Required Inspection Scope

Inspect metadata-level visibility only for:

- `/tmp`
- `/opt/codex`
- `/opt/codex/sessions`
- `/opt/codex/shell_snapshots`
- `/root/.cache`
- `/root/.npm/_logs`
- `/root/.local/share`
- active workspace root
- immediate parent workspace namespace, if accessible

The audit may inspect only:

- filenames;
- timestamps;
- inode metadata;
- file sizes;
- ownership/permission metadata;
- directory structure;
- artifact naming patterns;
- shell snapshot filenames;
- session directory naming structure;
- runtime chronology indicators;
- first line only of visible shell snapshot files, if present;
- first line only of visible session files, if present.

The audit must NOT:

- dump entire logs;
- print secrets;
- print tokens in full;
- print full session contents;
- print full shell snapshot contents;
- inspect beyond the first line of shell snapshot/session files;
- include raw first-line contents in the report;
- include raw commands, raw IDs, raw commit hashes, raw branch names, raw repository paths, raw usernames, raw hostnames, raw environment values, or raw instruction text;
- modify unrelated runtime locations;
- create or modify files other than the required report.

---

# First-Line Verification Requirements

For each visible shell snapshot or session artifact, inspect only the first line when safely accessible.

Determine whether the first line contains indicators of:

- repository path;
- command lineage;
- task/thread/session ID;
- unrelated execution context;
- embedded runtime instructions;
- source/origin metadata;
- model/provider metadata;
- git metadata.

The report must include only boolean or category-level findings.

Allowed examples:

```text
repository_path_indicator: present
command_lineage_indicator: present
task_thread_indicator: present
unrelated_context_indicator: not observed
embedded_instruction_indicator: present
source_origin_metadata_indicator: present
model_provider_metadata_indicator: present
git_metadata_indicator: present
```

Forbidden examples:

```text
cwd="/workspace/example"
id="019e..."
commit_hash="abc123..."
branch="work"
base_instructions="..."
```

Do not include raw first-line contents anywhere in the report.

---

# Cross-Task Canary Verification

If the user provides a benign canary marker for a prior task or controlled reproduction, the audit may search for the marker only in allowed metadata and first-line-only surfaces.

The audit must not create a new canary unless explicitly instructed by the user.

If a canary is used, it must be non-secret and researcher-controlled.

The report must redact the marker value and state only:

```text
controlled_canary_indicator: present
controlled_canary_indicator: not observed
```

Do not print the full canary value.

This section is intended to distinguish runtime metadata persistence from AGENTS.md instruction-precedence behavior.

---

# Redaction Requirements

All sensitive or correlation-capable values must be redacted.

Redact:

- tokens and secrets;
- session IDs;
- task/thread IDs;
- snapshot IDs;
- timestamps embedded in identifiers;
- repository paths;
- workspace paths;
- commands;
- commit hashes;
- branch names;
- usernames;
- hostnames;
- environment values;
- raw prompt text;
- raw instruction text;
- database row/content values.

Use stable redaction labels:

```text
[ID_REDACTED]
[TS_REDACTED]
[PATH_REDACTED]
[COMMAND_REDACTED]
[COMMIT_REDACTED]
[BRANCH_REDACTED]
[USER_REDACTED]
[HOST_REDACTED]
[ENV_REDACTED]
[INSTRUCTIONS_REDACTED]
[CANARY_REDACTED]
```

Short prefixes may be shown only when necessary for evidence, and must be limited to 4-6 characters:

```text
SESSION_ID=019e5...[REDACTED]
COMMIT=de001...[REDACTED]
```

Prefer full redaction unless a prefix is needed to demonstrate pattern continuity.

---

# Correlation Analysis Requirements

Determine whether visible artifacts contain indicators such as:

- historical timestamps;
- unrelated repository identifiers;
- unrelated workspace identifiers;
- thread identifiers;
- task identifiers;
- execution traces;
- orchestration metadata;
- session references;
- cache lineage indicators;
- build history indicators;
- historical execution remnants;
- runtime chronology indicators;
- shell snapshot persistence;
- runtime correlation IDs;
- execution lineage indicators;
- embedded runtime instruction indicators;
- git/source/model metadata indicators;
- connector/session isolation indicators.

---

# Artifact Classification

Classify observed artifacts as one or more of:

- current-task artifact;
- historical runtime artifact;
- cache/build artifact;
- execution-log artifact;
- orchestration/session artifact;
- shell-snapshot artifact;
- runtime-correlation artifact;
- unrelated runtime residue;
- potentially sensitive artifact.

---

# Required Deliverable

Create:

`WORKSPACE_ISOLATION_REPORT.md`

The report must contain:

- audit date;
- scope inspected;
- methodology;
- redaction controls applied;
- visible workspace namespace indicators;
- visible sibling repository or unrelated workspace indicators;
- visible runtime artifact locations;
- visible historical artifact indicators;
- visible cache/build artifact indicators;
- visible orchestration/session indicators;
- visible execution-trace indicators;
- visible shell snapshot indicators;
- visible runtime correlation identifiers;
- visible execution chronology indicators;
- first-line-only shell snapshot/session correlation results;
- controlled canary result, if a canary was provided;
- artifact classification table;
- whether artifacts appear related to:
  - the active task;
  - historical executions;
  - unrelated runtime activity;
- whether sibling repositories are visible;
- whether unrelated workspaces are visible;
- whether task/session residue is visible;
- whether shared runtime directories are visible;
- whether runtime residue appears historically persistent;
- whether shell snapshots appear historically retained;
- whether execution lineage indicators exist;
- whether runtime correlation identifiers appear reusable across executions;
- whether connector/session isolation appears preserved;
- whether broader runtime visibility indicators exist;
- whether environment isolation appears preserved;
- limitations encountered.

---

# Required Conclusions

The report must explicitly answer:

- Does any visible snapshot/session artifact contain a repository path indicator?
- Does any visible snapshot/session artifact contain command lineage?
- Does any visible snapshot/session artifact contain a task/thread/session ID?
- Does any visible snapshot/session artifact contain unrelated execution context?
- Does any visible snapshot/session artifact contain embedded runtime instructions?
- Does any visible snapshot/session artifact contain git metadata?
- Are sibling repositories visible?
- Are unrelated workspaces visible?
- Are shared runtime directories visible?
- Is historical runtime residue visible?
- Is shell snapshot persistence visible?
- Is orchestration/session persistence visible?
- Is reusable runtime state visible?
- Does connector/session isolation appear preserved?
- Does environment isolation appear preserved, partially preserved, or not preserved?

---

# Evidence Handling Rules

The report must be suitable for a high-security bug bounty submission.

Use real observations, but do not include raw sensitive values.

The report should be reproducible by describing:

- exact inspected locations;
- inspection depth;
- metadata fields checked;
- first-line-only verification method;
- redaction strategy;
- boolean/category-level indicator results.

The report must not include raw dumps of runtime files, raw session JSON, raw shell snapshot contents, raw environment variables, raw secrets, raw internal instructions, or raw command lines.

---

# Delivery Workflow

After generating the report, commit only the generated report:

```bash
git add WORKSPACE_ISOLATION_REPORT.md
git commit -m "Add workspace isolation and runtime governance audit report"
```
