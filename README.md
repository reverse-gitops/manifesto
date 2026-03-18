# The Reverse GitOps Manifesto

*An API-first operating model where users and agents make validated changes through a typed API, while a controller writes those changes back into Git as clean declarative intent for audit, promotion, and distribution.*

---

GitOps gave us reproducibility, auditability, and a shared language for intent. One of the best ideas the infrastructure community has produced.

But it carries an assumption that is starting to fail: that every user making a change should work directly through Git. Platform engineers are building self-service platforms. Developers and agents want a UI or a CLI backed by an API.

---

## The Problem With Git as a Front Door

Git was built for source code collaboration: line-oriented, text-based, and review-driven. These properties are virtues for code. They are friction for infrastructure operations. When Git is the front door, validation often happens after the fact: in CI, in controllers, or during deployment. Errors are discovered later, farther from the point of change.

Meanwhile, the Kubernetes API has spent ten years solving this problem with typed resources, validation at write time, and controllers built around structured state. Immediate feedback. A natural interface for humans and AI agents alike.

The better front door already exists.

---

## Principle 1: API First

A dedicated Kubernetes API is the front door for managing intent. Humans use it through CLIs, IDEs and GUIs. AI agents use it programmatically. Changes are validated immediately: wrong type, unknown field, policy violation, all caught at write time. Both humans and AI agents benefit from the tighter feedback loop.

A controller serializes API state back into Git as clean, declarative YAML files. The user's identity is preserved as commit author. This works because Kubernetes resources can be represented both as API objects and as declarative YAML. The controller's job is to capture intent cleanly, not reinterpret it. Git becomes the memory: every change recorded, diffable, auditable, without any user needing to write a commit.

---

## Principle 2: Capture Intent, Not Implementation

Users declare intent. The platform renders it. What gets preserved in Git must be the stable user-facing interface, not the concrete resources produced downstream.

Round-tripping is the test. The model is typed resources that can be validated, serialized, and restored as declarative state. User-facing abstractions should meet that same standard. Behind that boundary, the platform team retains full freedom: Helm, Kustomize, Crossplane, custom controllers. The constraint is on the interface, not the implementation.

---

## Principle 3: GitOps Still Applies

Reverse GitOps is a compliant extension of GitOps, not a replacement. Existing GitOps tools like Flux and ArgoCD continue to work as before. Git remains useful for promotion, disaster recovery, and bulk rollouts.

The OpenGitOps project defines four principles, and Reverse GitOps satisfies all four:

| OpenGitOps Principle | How Reverse GitOps satisfies it |
|---|---|
| Declarative | Resources submitted to the Kubernetes API are declarative by definition |
| Versioned and Immutable | A controller writes clean YAML commits to Git — every change is versioned |
| Pulled Automatically | The write-back controller pulls from the API; downstream GitOps operators pull from Git |
| Continuously Reconciled | Both the write-back controller and downstream operators reconcile continuously |

The GitOps spec notes that Git is the "canonical example" of a state store, not the only valid one. Reverse GitOps works today within that spec — and points toward where the ecosystem is heading.

---

The API comes first. Git remembers.

That is Reverse GitOps.

---

*First draft — March 2026. Feedback welcome.*
