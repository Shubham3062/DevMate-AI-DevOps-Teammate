# DevMate-AI-DevOps-Teammate

# Problem Statement

Nowadays majority of DevOps teams are increasing use of AI copilots to troubleshoot infrastructure, understand incidents and improve team productivity.

However, general-purpose AI assistants introduce three major problems in operational environments that we have seen already making headlines:

### 1. Stale or generic knowledge

AI models may provide technically correct information that does not reflect the organization's current infrastructure, deployment standards, architecture or operational procedures.

### 2. Lack of organizational context

A general-purpose copilot like chatgpt, claude does not inherently know which team owns a service, how production infrastructure is structured, what deployment process the organization follows, or what actions different engineering roles are permitted to perform.

### 3. Uncontrolled operational access

Giving an AI agent direct access to cloud infrastructure, Kubernetes clusters or CI/CD systems introduces security risks including unauthorized actions, privilege escalation, accidental destructive operations, credential exposure and insufficient auditability.

The result is a gap between **AI productivity** and **production-grade operational safety**.

DevMate addresses this gap by combining organization-specific knowledge, live infrastructure evidence, role-aware access control, governed MCP tools, reusable DevOps skills and auditable decision-making into a centralized AI DevOps teammate.

# Solution

DevMate is a governed AI DevOps teammate designed to assist DevOps teams with infrastructure troubleshooting and operational decision-making at any level whether it's a Junior engineer or a Cloud Architect.

Instead of relying solely on an LLM's general knowledge, DevMate combines five layers:

### Organization Knowledge (RAG)

A versioned knowledge base containing internal architecture, infrastructure standards, security policies, team ownership, deployment procedures and incident-management practices etc important information about the organization.

### Agent Skills

Structured troubleshooting procedures for technologies such as AWS, Kubernetes and Terraform (for demo purpose, willing to add more in the future). Skills define how the agent should investigate a problem in such a way that is aligned with an organizational manner rather than simply providing technical documentation.

### MCP tools

Model Context Protocol tools provide controlled access to live systems such as AWS, Kubernetes, GitHub and observability platforms.

### Policy Enforcement

RBAC and deterministic policies determine which operations a user and the agent are permitted to perform. The LLM is not treated as the security boundary.

### Evidence & Audit

The agent separates organizational facts, live infrastructure evidence, model inference and recommendations. Operational interactions are designed to be traceable and auditable.

The resulting workflow is:

User → Identity → Agent → Policy → Knowledge + Skills → MCP → Live Evidence → Decision → Audit

This allows DevMate to provide organization-aware answers while reducing the risks associated with generic AI recommendations and uncontrolled infrastructure access.

The project explores how AI agents can be integrated into DevOps environments without treating the LLM itself as a security boundary.

---

## Why DevMate?

General-purpose AI copilots are useful for explaining technologies and generating commands, but they do not inherently know:

* How an organization's infrastructure is structured
* Which team owns a service
* Which deployment procedures are approved
* What different engineering roles are allowed to do
* What is actually happening in the infrastructure right now

DevMate addresses this by combining:

**Organization Knowledge + Agent Skills + MCP Tools + RBAC + Policy + Evidence + Audit**

---

## Core Architecture

DevMate is built around seven architectural components.

<img width="1056" height="936" alt="DevMate" src="https://github.com/user-attachments/assets/94cde2b0-3af0-43e4-bbbf-192d1fb4fb53" />

### 1. RBAC

Determines what the requesting engineer is allowed to access.

Example:

```text
Junior Engineer
→ Read-only production investigation

Senior Engineer
→ Advanced operational access

Architect
→ Architecture and cross-system analysis
```

---

### 2. Organization Knowledge Base

Contains fictional enterprise documentation representing:

* Architecture
* AWS account standards
* Kubernetes standards
* Terraform standards
* Security policies
* Team ownership
* Deployment procedures
* Incident management
* Production access

The knowledge base is intentionally organization-specific rather than generic technical documentation.

---

### 3. Agent Skills

Skills define how the agent should investigate a problem.

Current skills:

```text
skills/
├── terraform-troubleshooting.md
├── kubernetes-troubleshooting.md
└── aws-troubleshooting.md
```

Planned:

```text
incident-response
security-troubleshooting
cost-anomaly-analysis
ci-cd-troubleshooting
```

---

### 4. MCP / Tool Layer

MCP and native integrations provide controlled access to external systems.

Initial tool domains:

```text
AWS
Kubernetes
Terraform
GitHub
```

The architecture intentionally avoids exposing unrestricted shell access to the agent.

Tools should expose narrow, auditable operations instead of arbitrary command execution.

---

### 5. Policy Engine

The LLM is not treated as the security boundary.

A deterministic policy layer evaluates:

```text
User Role
+
Environment
+
Requested Action
+
Risk Level
```

Example:

```text
Junior
+
Production
+
Delete Kubernetes Deployment
+
High Risk

→ DENY
```

The agent can still investigate the incident and provide a recommendation.

---

### 6. Evidence Layer

DevMate separates:

```text
Organizational Fact
Live Infrastructure Evidence
Model Inference
Recommendation
```

Example:

```text
ORGANIZATIONAL FACT
Production deployments require approval.

LIVE EVIDENCE
payments-api deployment 4.8.1 was deployed 14 minutes ago.

MODEL INFERENCE
The deployment may be related to the current failure.

RECOMMENDATION
Investigate configuration changes introduced in 4.8.1.
```

---

### 7. Audit

Important interactions should be traceable:

```text
User
Role
Request
Tools Used
Knowledge Retrieved
Policy Decision
Agent Decision
Action
Timestamp
Result
```

This provides the foundation for future production-grade observability and governance.

---

# Technology Stack

| Layer          | Technology                          |
| -------------- | ----------------------------------- |
| Agent Runtime  | SIM.ai                              |
| LLM            | SIM.ai supported model              |
| Agent Skills   | Markdown Files                      |
| Knowledge Base | Organizational Knowledge            |
| Vector Store   | SIM.ai managed / supported provider |
| Tool Protocol  | MCP                                 |
| Cloud          | AWS                                 |
| Infrastructure | Terraform                           |
| Containers     | Kubernetes                          |
| Source Control | GitHub                              |
| Observability  | CloudWatch / future integrations    |
| Security       | RBAC + Policy                       |
| Documentation  | Markdown files                      |

---

# Example: Organization-Aware Troubleshooting

Instead of asking:

> Why is my Kubernetes pod failing?

DevMate can investigate:

```text
User request
      ↓
Identify user role
      ↓
Retrieve organization standards
      ↓
Load Kubernetes troubleshooting skill
      ↓
Inspect live Kubernetes state
      ↓
Inspect logs/events
      ↓
Check recent deployment
      ↓
Compare against organizational policy
      ↓
Generate evidence-backed diagnosis
      ↓
Check whether remediation is permitted
      ↓
Recommend or execute
      ↓
Record audit information
```

---

# Example Security Scenario

### User

> Restart the production payments API.

### DevMate

```text
Role:
Junior Engineer

Environment:
Production

Requested action:
Restart deployment

Policy:
DENIED

Reason:
Junior engineers have read-only production access.

Allowed alternative:
Investigate deployment status, logs, events and recent changes.

Human escalation:
Required
```

The agent does not simply refuse the request. It provides the maximum useful assistance allowed by policy.

---

# Example Incident Scenario

### User

> Why is payments-api failing?

DevMate can correlate:

```text
Kubernetes
    ↓
Pod failures

CloudWatch
    ↓
Error spike

GitHub
    ↓
Recent deployment

Organization KB
    ↓
Deployment/configuration standard

Agent Skill
    ↓
Troubleshooting procedure

Policy
    ↓
Allowed actions

Evidence
    ↓
Root-cause hypothesis
```

The result is an organization-aware incident investigation rather than a generic LLM response.

---

# Design Principles

DevMate follows several core principles:

### 1. The LLM is not the security boundary

Authorization must be enforced outside the model.

### 2. Prefer evidence over assumptions

Live infrastructure evidence should take precedence over model knowledge when diagnosing current incidents.

### 3. Read before write

The agent should investigate before recommending or executing changes.

### 4. Least privilege

Tools should expose only the operations required for their purpose.

### 5. No unrestricted shell access

Infrastructure tools should use narrow, typed operations wherever possible.

### 6. Human approval for high-risk actions

Production and destructive operations require appropriate authorization.

### 7. Source-controlled knowledge

Organizational knowledge, skills and policies should be versioned.

### 8. Explain uncertainty

The agent should distinguish facts from hypotheses and recommendations.

---

# Current Status

```text
[x] Agent architecture
[x] Organization knowledge model
[x] Terraform troubleshooting skill
[x] Kubernetes troubleshooting skill
[x] AWS troubleshooting skill
[x] RBAC concept
[x] Policy concept
[x] Evidence model
[ ] MCP integrations
[ ] SIM.ai implementation
[ ] Evaluation suite
[ ] Security testing
[ ] Demo scenarios
[ ] Production architecture
```

---

# Project Goals

The goal of this project is not to build another AI chatbot.

The goal is to explore how AI agents can become useful DevOps teammates while maintaining:

* Security
* Organizational context
* Evidence-based reasoning
* Least privilege
* Auditability
* Low infrastructure cost
* Replaceable components

The project is intentionally designed so that the orchestration layer can eventually be replaced without rebuilding the underlying Skills, Knowledge, Policies, MCP interfaces and evaluation framework.

---

# Future Architecture

The current implementation uses SIM.ai for rapid prototyping.

A future production deployment could replace the hosted orchestration layer with a dedicated agent runtime and introduce:

```text
OIDC / IAM
MCP Gateway
Policy Enforcement
Managed Knowledge
Agent Evaluation
Observability
Audit Pipeline
CI/CD
```

The architecture therefore separates the **agent runtime** from the **DevOps intelligence and governance layers**.

---

# Disclaimer

This repository is an educational and portfolio project.

The organization, infrastructure, policies, accounts and operational data represented in the knowledge base are fictional and created for demonstration purposes.

Do not connect the example policies or tools directly to production infrastructure without implementing appropriate authentication, authorization, secrets management, auditing and operational controls.



