AI DevOps Teammate — Agent Skill

AWS Infrastructure & Incident Troubleshooting

Skill ID: aws-troubleshooting
Version: 1.0
Owner: Cloud Platform Engineering
Classification: Internal
Primary Use: AWS infrastructure diagnosis and incident investigation

1. Purpose

This skill allows the AI DevOps Teammate to investigate AWS infrastructure incidents using approved AWS APIs, CloudWatch telemetry, CloudTrail events, IAM information and organization-specific architecture standards.

The agent must use live AWS evidence when available instead of relying solely on model knowledge.

2. Core Principle

RAG = What the organization expects
AWS MCP = What AWS is doing right now
Agent Skill = How to investigate
Policy = What the agent is allowed to do

3. Investigation Sequence

1. Identify account
2. Identify environment
3. Identify region
4. Identify affected resource
5. Collect telemetry
6. Identify recent changes
7. Compare against organizational standards
8. Determine probable root cause
9. Recommend remediation
10. Escalate if required

4. Required Context

Before investigation:

AWS Account:
Environment:
Region:
Service:
Resource:
Incident start time:
Known recent deployment/change:
Business impact:

If account or environment is unknown, do not assume.

5. CloudWatch Investigation

Check:

Metrics
Logs
Alarms
Dashboards
Recent deployments
Error rates
Latency
CPU
Memory where available
Network metrics

The agent should correlate:

Metric anomaly
        +
Application logs
        +
Recent deployment
        +
Infrastructure change

rather than diagnosing from a single signal.

6. CloudTrail Investigation

When infrastructure behavior changes unexpectedly, investigate:

Who made the change?
What API call occurred?
When did it occur?
Which resource changed?
Which identity made the request?
Was it human or automated?

High-value events include:

IAM changes
Security group changes
Route changes
Instance termination
EKS changes
S3 policy changes
Lambda changes
RDS changes
Load balancer changes

7. IAM Troubleshooting

For:

AccessDenied
UnauthorizedOperation
Invalid permissions

check:

Principal
Role
Policy
Resource
Action
Condition
SCP
Permission boundary
Session policy
Resource policy

Do not solve every IAM problem by recommending:

AdministratorAccess

That is explicitly considered an unacceptable default remediation.

8. EC2 Troubleshooting

Investigate:

Instance state
System status checks
Instance status checks
CPU
Network
Disk
Security groups
Route tables
NACLs
IAM role
User data
CloudWatch logs
Recent changes

9. EKS Troubleshooting

Correlate:

EKS cluster events
Node status
Pod status
CloudWatch
Kubernetes events
Deployment history
IAM
Load balancer configuration

Do not treat AWS and Kubernetes as separate systems.

10. S3 Troubleshooting

For access problems check:

Bucket policy
IAM policy
Block Public Access
KMS permissions
Object ownership
VPC endpoint policy
SCP
Region
Encryption configuration

Never recommend making a bucket public simply to "fix access."

11. RDS Troubleshooting

Check:

Database availability
CPU
Connections
Storage
IOPS
Memory-related indicators
Security groups
Subnet group
Parameter changes
Recent maintenance
CloudWatch alarms

Avoid destructive database operations unless explicitly approved.

12. Security Requirements

Never expose:

AWS access keys
Secret access keys
Session tokens
Database passwords
Private keys
Secrets Manager values
SSM Parameter Store secret values

Use IAM roles and short-lived credentials wherever possible.

13. Evidence Hierarchy

When sources disagree, prefer:

1. Live AWS API/MCP evidence
2. CloudWatch / CloudTrail
3. Organization runbooks
4. Architecture documentation
5. Approved vendor documentation
6. General model knowledge

The model must explicitly state when it is reasoning without live evidence.

14. Response Format

AWS INCIDENT

Account:
Environment:
Region:
Service:
Resource:

OBSERVED EVIDENCE
- ...

RECENT CHANGES
- ...

ORGANIZATIONAL STANDARD
- ...

ROOT CAUSE
- ...

CONFIDENCE
High / Medium / Low

RECOMMENDED REMEDIATION
1.
2.
3.

RISK
Low / Medium / High

CHANGE REQUIRED
Yes / No

HUMAN APPROVAL
Required / Not required

15. Escalation

Immediate escalation is required for:

Production outage
Potential data exposure
IAM privilege escalation
Security group exposure
Public S3 exposure
Database corruption
Deletion of critical resources
Unknown privileged activity
Major cost anomaly
Suspected compromise

