# Kubernetes Manifest Validation and Troubleshooting Lab

## Overview

This repository contains a junior-level Kubernetes troubleshooting lab focused on identifying, fixing, and validating common Kubernetes manifest issues.

The purpose of this project is to practice reading Kubernetes YAML files, identifying configuration problems, documenting root causes, and validating corrected manifests with GitHub Actions.

This project is part of my Junior DevOps portfolio and focuses on Kubernetes troubleshooting, YAML validation, `kubectl` usage, GitHub Actions validation, and root-cause documentation.

---

## Scenario

A development team is preparing application manifests for Kubernetes deployment.

Before these manifests can be used in a live environment, several common configuration issues need to be identified and corrected. The goal is to review the broken manifests, explain the root cause, provide corrected versions, and validate the fixed manifests through an automated GitHub Actions workflow.

This project does not deploy workloads to a live Kubernetes cluster. It focuses on manifest troubleshooting and pre-deployment validation.

---

## Objectives

* Identify common Kubernetes manifest issues
* Compare broken and fixed Kubernetes YAML files
* Troubleshoot Service selector mismatches
* Troubleshoot invalid image references
* Troubleshoot Service and container port mismatches
* Troubleshoot missing ConfigMap references
* Validate YAML syntax
* Validate fixed manifests with `kubectl dry-run`
* Automate manifest validation with GitHub Actions
* Document root cause, impact, and resolution for each issue

---

## Environment

| Component            | Details             |
| -------------------- | ------------------- |
| Version Control      | Git and GitHub      |
| Automation           | GitHub Actions      |
| Kubernetes Tooling   | kubectl             |
| Configuration Format | YAML                |
| Validation Method    | Client-side dry-run |
| Cluster Required     | No                  |

---

## Tools and Commands Used

| Tool / Command                    | Purpose                                         |
| --------------------------------- | ----------------------------------------------- |
| `kubectl create --dry-run=client` | Validate Kubernetes manifests without deploying |
| `python yaml.safe_load()`         | Validate YAML syntax                            |
| `GitHub Actions`                  | Run automated validation workflow               |
| `git`                             | Version control and repository management       |
| `YAML`                            | Kubernetes manifest format                      |

---

## Skills Demonstrated

* Kubernetes manifest troubleshooting
* YAML syntax validation
* Service selector analysis
* Deployment and Service relationship troubleshooting
* Image reference troubleshooting
* ConfigMap reference troubleshooting
* `kubectl` dry-run validation
* GitHub Actions workflow automation
* Root-cause analysis
* Technical documentation

---

## Repository Structure

```text
kubernetes-manifest-validation-troubleshooting-lab/
│
├── README.md
│
├── scenarios/
│   ├── 01-service-selector-mismatch/
│   │   ├── broken/
│   │   │   ├── deployment.yaml
│   │   │   └── service.yaml
│   │   └── fixed/
│   │       ├── deployment.yaml
│   │       └── service.yaml
│   │
│   ├── 02-invalid-image-reference/
│   │   ├── broken/
│   │   │   └── deployment.yaml
│   │   └── fixed/
│   │       └── deployment.yaml
│   │
│   ├── 03-port-mismatch/
│   │   ├── broken/
│   │   │   ├── deployment.yaml
│   │   │   └── service.yaml
│   │   └── fixed/
│   │       ├── deployment.yaml
│   │       └── service.yaml
│   │
│   └── 04-missing-configmap/
│       ├── broken/
│       │   └── deployment.yaml
│       └── fixed/
│           ├── configmap.yaml
│           └── deployment.yaml
│
├── .github/
│   └── workflows/
│       └── validate-k8s-manifests.yml
│
└── notes/
    ├── commands-used.md
    └── troubleshooting-summary.md
```

---

## Troubleshooting Scenarios

### Scenario 1: Service Selector Mismatch

**Problem:**
The Service selector does not match the labels on the Pods created by the Deployment.

**Impact:**
The Service cannot route traffic to the application Pods.

**Root Cause:**
The Service is selecting the wrong label.

**Resolution:**
Update the Service selector so it matches the Pod labels from the Deployment.

---

### Scenario 2: Invalid Image Reference

**Problem:**
The Deployment references an invalid container image tag.

**Impact:**
In a live cluster, the Pod may fail with `ImagePullBackOff` or `ErrImagePull`.

**Root Cause:**
The image tag does not exist.

**Resolution:**
Update the Deployment to use a valid image tag.

---

### Scenario 3: Service and Container Port Mismatch

**Problem:**
The Service `targetPort` does not match the container port used by the application.

**Impact:**
The Service may exist, but traffic will not reach the application correctly.

**Root Cause:**
The Service forwards traffic to the wrong target port.

**Resolution:**
Update the Service `targetPort` to match the container port.

---

### Scenario 4: Missing ConfigMap Reference

**Problem:**
The Deployment references a ConfigMap that does not exist in the broken manifest set.

**Impact:**
In a live cluster, the Pod may fail to start or report configuration errors.

**Root Cause:**
The required ConfigMap was not created.

**Resolution:**
Create the missing ConfigMap and keep the Deployment reference consistent.

---

## GitHub Actions Validation Workflow

The automated workflow validates the corrected manifests.

The workflow runs on:

* Push to `main`
* Pull request to `main`

Validation steps:

```text
Checkout repository
        |
        v
Verify scenario directories exist
        |
        v
Validate YAML syntax
        |
        v
Install kubectl
        |
        v
Validate fixed manifests with kubectl dry-run
```

Workflow file:

```text
.github/workflows/validate-k8s-manifests.yml
```

---

## Validation Commands

The fixed manifests can be validated without deploying to a cluster.

```bash
kubectl create --dry-run=client --validate=false -f scenarios/01-service-selector-mismatch/fixed/ -o yaml
kubectl create --dry-run=client --validate=false -f scenarios/02-invalid-image-reference/fixed/ -o yaml
kubectl create --dry-run=client --validate=false -f scenarios/03-port-mismatch/fixed/ -o yaml
kubectl create --dry-run=client --validate=false -f scenarios/04-missing-configmap/fixed/ -o yaml
```

---

## Verification Checklist

* [ ] Broken manifests are included for each scenario
* [ ] Fixed manifests are included for each scenario
* [ ] Root cause is documented for each issue
* [ ] GitHub Actions workflow is included
* [ ] YAML syntax validation passes
* [ ] Fixed manifests pass `kubectl dry-run` validation
* [ ] Troubleshooting notes are documented
* [ ] Repository structure is organized clearly

---

## Lessons Learned

This project reinforced that many Kubernetes issues are caused by small configuration mismatches.

The most important troubleshooting pattern was comparing what one resource expects against what another resource provides. For example, a Service must select labels that actually exist on Pods, and a Service `targetPort` should match the port exposed by the container.

This project also demonstrated how GitHub Actions can be used as a pre-deployment quality gate to validate Kubernetes manifests before they are applied to a live environment.

---

## Portfolio Note

This project is part of my Junior DevOps portfolio. It demonstrates Kubernetes troubleshooting, manifest validation, YAML review, GitHub Actions automation, and root-cause documentation.
