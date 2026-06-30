# Kubernetes Manifest Validation and Troubleshooting Lab

## Overview

This repository contains a junior-level Kubernetes troubleshooting lab focused on identifying, fixing, and validating common Kubernetes manifest issues.

The purpose of this project is to practice reading Kubernetes YAML files, identifying configuration problems, documenting root causes, and validating corrected manifests with GitHub Actions.

This project is part of my Junior DevOps portfolio and focuses on Kubernetes troubleshooting, YAML validation, Kubernetes manifest review, GitHub Actions validation, and root-cause documentation.

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
* Validate fixed Kubernetes manifests without a live cluster
* Automate manifest validation with GitHub Actions
* Document root cause, impact, and resolution for each issue

---

## Environment

| Component             | Details                                      |
| --------------------- | -------------------------------------------- |
| Version Control       | Git and GitHub                               |
| Automation            | GitHub Actions                               |
| Kubernetes Validation | kubeconform                                  |
| Configuration Format  | YAML                                         |
| Validation Method     | Schema validation and YAML syntax validation |
| Cluster Required      | No                                           |

---

## Tools and Commands Used

| Tool / Command            | Purpose                                                   |
| ------------------------- | --------------------------------------------------------- |
| `git`                     | Version control and repository management                 |
| `GitHub Actions`          | Runs automated validation workflow                        |
| `python yaml.safe_load()` | Validates YAML syntax                                     |
| `kubeconform`             | Validates Kubernetes manifests against Kubernetes schemas |
| `YAML`                    | Kubernetes manifest format                                |

---

## Skills Demonstrated

* Kubernetes manifest troubleshooting
* YAML syntax validation
* Kubernetes schema validation
* Service selector analysis
* Deployment and Service relationship troubleshooting
* Image reference troubleshooting
* ConfigMap reference troubleshooting
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
Install Python YAML dependency
        |
        v
Validate YAML syntax
        |
        v
Install kubeconform
        |
        v
Validate fixed manifests with kubeconform
```

Workflow file:

```text
.github/workflows/validate-k8s-manifests.yml
```

---

## Validation Method

This project originally attempted to use `kubectl dry-run` for validation. However, GitHub Actions does not have access to a Kubernetes API server by default.

Because this lab is intentionally clusterless, the workflow now uses `kubeconform` to validate Kubernetes manifests against Kubernetes schemas without needing a live Kubernetes cluster.

This makes the project better aligned with its purpose: pre-deployment manifest validation.

---

## Example Workflow Validation Commands

The workflow validates the fixed manifests using `kubeconform`:

```bash
kubeconform -summary -strict scenarios/01-service-selector-mismatch/fixed/
kubeconform -summary -strict scenarios/02-invalid-image-reference/fixed/
kubeconform -summary -strict scenarios/03-port-mismatch/fixed/
kubeconform -summary -strict scenarios/04-missing-configmap/fixed/
```

---

## Verification Checklist

* [ ] Broken manifests are included for each scenario
* [ ] Fixed manifests are included for each scenario
* [ ] Root cause is documented for each issue
* [ ] GitHub Actions workflow is included
* [ ] YAML syntax validation passes
* [ ] Fixed manifests pass kubeconform validation
* [ ] Troubleshooting notes are documented
* [ ] Repository structure is organized clearly
* [ ] GitHub Actions workflow completes successfully

---

## Lessons Learned

This project reinforced that many Kubernetes issues are caused by small configuration mismatches.

The most important troubleshooting pattern was comparing what one resource expects against what another resource provides. For example, a Service must select labels that actually exist on Pods, and a Service `targetPort` should match the port exposed by the container.

This project also demonstrated how GitHub Actions can be used as a pre-deployment quality gate to validate Kubernetes manifests before they are applied to a live environment.

---

## Portfolio Note

This project is part of my Junior DevOps portfolio. It demonstrates Kubernetes troubleshooting, manifest validation, YAML review, GitHub Actions automation, and root-cause documentation.
