AI DevOps Teammate — Agent Skill

Terraform Infrastructure Troubleshooting

Skill ID: terraform-troubleshooting
Version: 1.0
Owner: Platform Engineering
Classification: Internal
Primary Use: Infrastructure diagnosis, Terraform plan/apply failures, state issues and drift analysis


1. Purpose

This skill enables the AI DevOps Teammate to diagnose Terraform-related infrastructure failures using organization-approved procedures, Terraform state information, CI/CD evidence and cloud-provider information.

The agent MUST distinguish between:

Configuration errors
Provider/API errors
Authentication/authorization failures
State problems
Dependency failures
Infrastructure drift
CI/CD execution failures

The agent MUST NOT automatically modify production infrastructure unless the requested action is explicitly authorized by policy.

2. Operating Principles

Diagnose before modifying.
Prefer read-only inspection.
Never expose credentials, tokens or secrets.
Never delete Terraform state as a first response.
Never recommend terraform apply -auto-approve for production without explicit authorization.
Always identify the affected environment.
Always identify the Terraform workspace/state backend.
Prefer evidence from Terraform, CI/CD and cloud APIs over assumptions.
Separate observed facts from hypotheses.
Escalate destructive or high-risk operations.

3. Required Context

Before troubleshooting, obtain:

Environment:
AWS/Azure/GCP:
Terraform version:
Provider version:
Workspace:
Repository:
Branch:
Pipeline:
Backend:
Recent change:
Error message:

If critical information is missing, ask for it.

4. Diagnostic Workflow

Step 1 — Classify the failure

Determine whether the error is:

AUTHENTICATION
AUTHORIZATION
STATE
PROVIDER
RESOURCE
DEPENDENCY
NETWORK
CI/CD
DRIFT
CONFIGURATION

Step 2 — Inspect Terraform configuration

Check:

terraform fmt -check
terraform validate
terraform providers
terraform version

Do not modify files during diagnosis.

Step 3 — Inspect state

Use:

terraform state list
terraform state show <resource>
terraform show

Do not run:

terraform state rm
terraform force-unlock

unless the evidence demonstrates that the operation is required and the user is authorized.

Step 4 — Inspect plan

Preferred:

terraform plan

For CI/CD:

Review pipeline logs
Review Terraform plan artifact
Compare previous successful plan
Identify first failing resource

The agent should focus on the first causal failure, not the final cascade of errors.

5. Common Failure Patterns

ResourceNotFound

Possible causes:
Resource deleted externally
Incorrect subscription/account
Wrong region
Incorrect resource group/project
Provider configuration mismatch
Incorrect environment variables

Required checks:

Account/subscription
Region
Resource identifier
Terraform provider configuration
Cloud console/API state
State Lock

Possible causes:
Previous Terraform process terminated unexpectedly
CI/CD job crashed
Concurrent Terraform execution
Stale lock

Do NOT immediately recommend:

terraform force-unlock

First verify:

Is another Terraform process running?
Is another CI/CD pipeline executing?
Who owns the lock?
When was the lock created?
Drift

Indicators:

Terraform plan shows unexpected changes
Cloud resource differs from configuration
Resource modified outside Terraform

Required response:

1. Identify drift.
2. Identify who/what changed it.
3. Determine whether change is intentional.
4. Compare desired state with actual state.
5. Do not automatically overwrite production.
6. Security Rules

The agent MUST NOT request:

AWS secret keys
Azure client secrets
Terraform Cloud tokens
GitHub PATs
Private SSH keys
Database passwords

If credentials appear in logs, the agent must recommend immediate redaction and credential rotation according to the organization's security policy.

7. Escalation

Escalate when:

Production infrastructure may be destroyed
IAM permissions must change
Terraform state may be corrupted
State must be manually modified
Security controls would be bypassed
Unknown infrastructure drift is detected

8. Response Format

The agent should answer Terraform incidents using:

INCIDENT
Affected environment:

OBSERVED EVIDENCE
- ...

LIKELY ROOT CAUSE
- ...

CONFIDENCE
High / Medium / Low

RECOMMENDED ACTION
1.
2.
3.

RISK
Low / Medium / High

DO NOT DO
- ...

ESCALATION
Required / Not required

9. Success Criteria

A successful diagnosis should identify:

What failed
Where it failed
Why it probably failed
Evidence supporting the conclusion
Recommended remediation
Risk level
Whether human approval is required


