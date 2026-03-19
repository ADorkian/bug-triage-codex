# Triage Rubric

This rubric defines how the `triage-analyst` agent scores incoming bug reports.

---

## 1. Severity

Severity describes the **impact** of the bug on users or the system.

| Level | Label | Description |
|---|---|---|
| 1 | **critical** | System is down or data is being lost / corrupted. No workaround exists. All users are affected. |
| 2 | **high** | Core functionality is broken for a significant subset of users. A workaround may exist but is cumbersome. |
| 3 | **medium** | A feature behaves incorrectly but a reasonable workaround exists. Limited user impact. |
| 4 | **low** | Minor issue: cosmetic, rarely encountered edge case, or minor performance degradation. |
| 5 | **trivial** | Typo, wording improvement, or non-functional issue with no user impact. |

### Escalation Rules

- If the bug involves **authentication, authorization, or data exposure**, automatically elevate severity by one level.
- If the bug is reported by a **premium/enterprise customer**, record the elevation in `reasoning` but do not automatically change the level (human review required).

---

## 2. Priority

Priority describes the **urgency** of addressing the bug.

| Label | Meaning | Default SLA |
|---|---|---|
| **P0** | Drop everything — fix now | 4 hours |
| **P1** | Fix in the current sprint | 2 business days |
| **P2** | Fix in the next sprint | 5 business days |
| **P3** | Scheduled | Next available window |
| **P4** | Backlog | No SLA |

### Severity → Priority Mapping (default)

| Severity | Default Priority |
|---|---|
| critical | P0 |
| high | P1 |
| medium | P2 |
| low | P3 |
| trivial | P4 |

The `triage-analyst` may deviate from the default mapping when there is strong evidence (e.g. very low frequency for a critical path bug).  Any deviation must be recorded in `reasoning`.

---

## 3. Estimated Effort

Effort estimates use T-shirt sizes.

| Label | Approximate scope |
|---|---|
| **XS** | < 1 hour — trivial one-liner or config change |
| **S** | 1–4 hours — small, well-understood change |
| **M** | 4–8 hours (one day) — moderate change touching a few files |
| **L** | 1–3 days — larger change requiring design decisions |
| **XL** | > 3 days — significant refactor or feature work |

---

## 4. Confidence

| Level | Meaning |
|---|---|
| **high** | Issue description is clear, root cause is evident from code context, fix is straightforward. |
| **medium** | Some ambiguity in the issue description or code context; hypothesis is reasonable but may need developer verification. |
| **low** | Insufficient information to be confident; a developer should investigate before proceeding. |

---

## 5. Worked Example

**Issue:** "Login button does nothing on mobile Chrome after the v3.2 release."

| Field | Value | Reasoning |
|---|---|---|
| severity | high | Login is core functionality; affects mobile Chrome users (significant subset). |
| priority | P1 | Aligns with default mapping for high severity. |
| affected_component | auth/login | Determined from code-context bundle. |
| estimated_effort | S | Likely a CSS or event-handler regression introduced in v3.2. |
| confidence | medium | No stack trace provided; hypothesis based on timing (v3.2 release). |
