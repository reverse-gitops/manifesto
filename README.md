# The Reverse GitOps Manifesto

*An API-first operating model where users and agents submit validated intent through a typed API, while that intent is automatically recorded in Git as clean declarative YAML manifests for audit, promotion, and distribution.*

---

## Perceived Problem

GitOps gives us reproducibility, auditability, and a shared language for intent. One of the best ideas the infrastructure community has produced.

But it carries an assumption that is starting to strain: that every change should enter the system through Git.

Git was built for source code collaboration. That works well for code. For infrastructure operations, the friction is specific:

- **Feedback is slow.** In the best case, a CI step validates schema. That is not common. Most of the time the operator is the first to know something is wrong, long after the change was written, reviewed, and merged.
- **Clients lack platform context.** A tool or agent generating a manifest cannot always know which resource versions, policies, and admission rules apply until validation happens against the real platform.
- **Git is not always the right write interface.** It is excellent for history, review, and distribution, but less natural as the primary interface for automation, non-engineers, or agents expressing intent programmatically.
- **Templating becomes the workaround.** When Git is the front door, teams layer overlays to manage repetition. Not always wrong, but often a symptom.

---

## Principle 1: API First

A dedicated Kubernetes API is the front door for intent: which applications to run, which routes are allowed, which teams have access. Not workloads, not rendered infrastructure. This API is deliberately shaped around stable user-facing abstractions, not arbitrary cluster resources. Humans use it through CLIs, IDEs, and GUIs. AI agents use it programmatically. Changes are validated immediately: wrong type, unknown field, policy violation, all caught at write time.

Relevant API activity is automatically written to Git as YAML. An HTTP POST creates a new manifest, a PATCH updates it, a DELETE removes it. The actors identity is preserved as commit author. A defined section of the repository (the intent folder) is managed automatically, the rest left untouched. Git becomes the memory: every change recorded, diffable, auditable, without any actor needing to write a commit.

This only works for user-facing resources whose declarative form is stable enough to round-trip cleanly.

---

## Principle 2: Capture Intent, Not Implementation

The platform team defines the CRDs that represent user intent: an Application, a HelmRelease, a DatabaseClaim, or other user-facing abstractions suited to the platform. What gets written to Git are those resources. Reconstruction from rendered output is in most situations impossible, and not the goal.

Behind the intent boundary, the platform team retains full freedom: Helm, Kustomize, Crossplane, custom controllers. The constraint is on the interface, not the implementation. From there, existing GitOps tools like Flux and ArgoCD take over: Git remains the layer for promotion, disaster recovery, and bulk rollouts.

---

## Principle 3: GitOps Still Applies

This pattern sits *upstream* of GitOps, not against it.

Direct Git edits remain valid too. The API is the preferred front door for intent, not the only one.

Git is the "canonical example" of a state store, not the only valid one. Distribution through signed OCI artifacts fits this pattern just as well.

---

## Non-goals

* Reverse GitOps does not claim that arbitrary live cluster state can always be reconstructed into clean source configuration.

* It does not remove the need for GitOps operators such as Flux or ArgoCD.

* It does not restrict how platform teams implement abstractions behind the user-facing boundary.


---

The API comes first. Git remembers.

That is Reverse GitOps.

---

*Second draft - April 2026. Feedback welcome.*
