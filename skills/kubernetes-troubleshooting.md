AI DevOps Teammate — Agent Skill

Kubernetes Troubleshooting

Skill ID: kubernetes-troubleshooting
Version: 1.0
Owner: Platform Engineering
Classification: Internal
Primary Use: Kubernetes workload, networking, scheduling and cluster troubleshooting

1. Purpose

This skill provides a standardized procedure for diagnosing Kubernetes incidents using cluster state, events, logs, workload configuration and organization-specific operational standards.

The agent must prioritize evidence over generic Kubernetes advice.

2. Safety Model

Read-only commands are preferred

Examples:

kubectl get pods -A
kubectl get nodes
kubectl get events -A
kubectl describe pod <pod>
kubectl logs <pod>
kubectl get deployment <deployment> -o yaml
kubectl get svc
kubectl get ingress

The agent must NOT execute destructive operations automatically.

Examples requiring explicit authorization:

kubectl delete pod
kubectl delete deployment
kubectl rollout undo
kubectl cordon node
kubectl drain node
kubectl delete namespace

3. Incident Classification

Classify the incident as:

POD FAILURE
DEPLOYMENT FAILURE
NODE FAILURE
SCHEDULING
NETWORKING
SERVICE DISCOVERY
INGRESS
CONFIGURATION
SECRET/CONFIGMAP
RESOURCE PRESSURE
STORAGE
RBAC
APPLICATION

4. Standard Investigation

Step 1 — Determine scope

Identify:

Cluster:
Namespace:
Application:
Deployment:
Pod:
Node:
Environment:

Never assume the cluster or namespace.

Step 2 — Check workload health

kubectl get pods -n <namespace>
kubectl get deployment -n <namespace>
kubectl get replicasets -n <namespace>

Look for:

CrashLoopBackOff
ImagePullBackOff
Pending
OOMKilled
Error
ContainerCreating
Readiness probe failures
Liveness probe failures

5. CrashLoopBackOff

Investigation:

kubectl describe pod <pod>
kubectl logs <pod>
kubectl logs <pod> --previous
kubectl get events -n <namespace>

Check:

Application crash
Environment variables
Secrets
ConfigMaps
Volume mounts
Dependency availability
Resource limits
Health probes
Exit codes

Do not immediately recommend increasing resources.

6. ImagePullBackOff

Check:

Image name
Image tag
Registry availability
ImagePullSecrets
IAM permissions
Network connectivity
Image existence

Potential root causes:

Wrong image tag
Private registry authentication failure
Deleted image
Incorrect registry URL
Expired credentials
Node network failure

7. Pending Pods

Check:

kubectl describe pod <pod>
kubectl get nodes
kubectl describe node <node>

Investigate:

Insufficient CPU
Insufficient memory
Node selectors
Taints
Tolerations
Affinity rules
PVC availability
Scheduling constraints

8. Service Networking

Check:

kubectl get svc -n <namespace>
kubectl get endpoints -n <namespace>
kubectl get endpointslices -n <namespace>

Validate:

Service selector
Pod labels
Target port
Container port
NetworkPolicy
Ingress configuration
DNS

A service with no endpoints should immediately trigger inspection of:

Pod labels
Service selector
Pod readiness

9. Node Problems

Check:

kubectl get nodes
kubectl describe node <node>
kubectl get events --sort-by=.lastTimestamp

Look for:

NotReady
DiskPressure
MemoryPressure
PIDPressure
NetworkUnavailable
Container runtime failures

The agent must correlate node-level symptoms with affected workloads before recommending remediation.

10. Security Rules

Never request or expose:

Kubernetes Secrets
Service account tokens
Cloud credentials
Private keys
Registry passwords

If secret content is required for diagnosis, ask for:

Secret key names
Presence/absence
Metadata
Non-sensitive error output

Never ask the user to paste secret values.

11. Response Format

KUBERNETES INCIDENT

Cluster:
Namespace:
Workload:

SYMPTOMS
- ...

EVIDENCE
- ...

ROOT CAUSE
- ...

CONFIDENCE
High / Medium / Low

RECOMMENDED ACTION
1.
2.
3.

RISK
Low / Medium / High

PRODUCTION IMPACT
None / Potential / Active

HUMAN APPROVAL
Required / Not required

12. Escalation Conditions

Escalate when:

Production cluster is affected
Node replacement is required
RBAC must change
Secrets must change
Network policies must change
Persistent volumes may be affected
Multiple workloads are failing
Root cause cannot be established confidently


