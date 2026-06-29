# Kubernetes Troubleshooting Summary

## Purpose of This File

This file summarizes the troubleshooting scenarios included in this lab.

Each scenario contains:

* The problem being demonstrated
* The expected impact in a real Kubernetes environment
* The root cause of the issue
* The corrected configuration
* The troubleshooting lesson learned

The goal is to practice reading Kubernetes manifests, identifying configuration mistakes, and explaining the fix clearly.

---

# Scenario 1: Service Selector Mismatch

## Description

This scenario demonstrates what happens when a Kubernetes Service selector does not match the labels on the Pods created by a Deployment.

A Service uses labels to find the Pods it should send traffic to. If the selector does not match the Pod labels, the Service will not have any matching backend Pods.

## Problem

The Service is configured with the wrong selector.

Broken Service selector:

```yaml
selector:
  app: wrong-app-label
```

But the Pods created by the Deployment use this label:

```yaml
labels:
  app: selector-demo-app
```

## Impact

In a live Kubernetes cluster, the Service would exist, but it would not route traffic to the application Pods.

This could make the application unreachable even though the Pods are running.

## Root Cause

The Service selector does not match the Pod labels.

## Fix

Update the Service selector so it matches the labels used by the Deployment’s Pod template.

Fixed Service selector:

```yaml
selector:
  app: selector-demo-app
```

## Troubleshooting Lesson

When a Service cannot reach Pods, check whether the Service selector matches the Pod labels.

Useful commands in a live cluster would include:

```bash
kubectl get pods --show-labels
kubectl describe service selector-demo-service
kubectl get endpoints selector-demo-service
```

---

# Scenario 2: Invalid Image Reference

## Description

This scenario demonstrates what happens when a Deployment references a container image tag that does not exist.

Kubernetes must be able to pull the container image before it can start a Pod successfully.

## Problem

The broken Deployment uses an invalid image tag.

Broken image reference:

```yaml
image: nginx:not-a-real-tag
```

## Impact

In a live Kubernetes cluster, the Pod would likely fail with one of these errors:

```text
ImagePullBackOff
```

or:

```text
ErrImagePull
```

The Deployment would not become healthy because Kubernetes could not pull the requested image.

## Root Cause

The image tag does not exist in the container registry.

## Fix

Use a valid container image tag.

Fixed image reference:

```yaml
image: nginx:alpine
```

## Troubleshooting Lesson

When Pods are stuck in `ImagePullBackOff` or `ErrImagePull`, check the image name, image tag, registry path, and image pull permissions.

Useful commands in a live cluster would include:

```bash
kubectl get pods
kubectl describe pod POD_NAME
kubectl get events
```

---

# Scenario 3: Service and Container Port Mismatch

## Description

This scenario demonstrates what happens when a Kubernetes Service forwards traffic to the wrong container port.

The Service `targetPort` should match the port where the application container is actually listening.

## Problem

The container exposes port `80`.

Deployment container port:

```yaml
containerPort: 80
```

But the broken Service forwards traffic to port `8080`.

Broken Service targetPort:

```yaml
targetPort: 8080
```

## Impact

In a live Kubernetes cluster, the Service may exist and may select the correct Pods, but traffic would not reach the application correctly.

This could cause connection failures or an unreachable application.

## Root Cause

The Service `targetPort` does not match the container port used by the application.

## Fix

Update the Service `targetPort` to match the container port.

Fixed Service targetPort:

```yaml
targetPort: 80
```

## Troubleshooting Lesson

When a Service selects the correct Pods but the application is still unreachable, check the Service `port`, `targetPort`, and the container’s listening port.

Useful commands in a live cluster would include:

```bash
kubectl describe service port-demo-service
kubectl get pods -o wide
kubectl describe pod POD_NAME
```

---

# Scenario 4: Missing ConfigMap Reference

## Description

This scenario demonstrates what happens when a Deployment references a ConfigMap that does not exist.

Applications often use ConfigMaps for environment variables or non-sensitive configuration. If the referenced ConfigMap is missing, the Pod may fail to start correctly.

## Problem

The Deployment references a ConfigMap named `app-config`.

Deployment reference:

```yaml
envFrom:
  - configMapRef:
      name: app-config
```

But the broken manifest set does not include the ConfigMap.

## Impact

In a live Kubernetes cluster, the Pod may fail to start or report configuration-related errors.

## Root Cause

The Deployment expects a ConfigMap named `app-config`, but that ConfigMap was not created.

## Fix

Create the missing ConfigMap.

Fixed ConfigMap:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  APP_ENV: "demo"
  APP_OWNER: "devops"
```

## Troubleshooting Lesson

When a Pod references external configuration, verify that the required ConfigMap or Secret exists in the same namespace.

Useful commands in a live cluster would include:

```bash
kubectl get configmaps
kubectl describe deployment configmap-demo-app
kubectl describe pod POD_NAME
```

---

# Overall Troubleshooting Workflow

This lab follows a simple Kubernetes troubleshooting workflow:

1. Review the broken manifest.
2. Identify the expected relationship between resources.
3. Compare related fields such as labels, selectors, ports, image references, and ConfigMap names.
4. Identify the root cause.
5. Review the corrected manifest.
6. Validate the fixed manifests using GitHub Actions and `kubectl dry-run`.

---

# Key Takeaways

Many Kubernetes issues are caused by small configuration mismatches.

The most important troubleshooting patterns from this lab are:

* Services must select labels that actually exist on Pods.
* Container image names and tags must be valid.
* Service `targetPort` should match the application’s container port.
* Deployments that reference ConfigMaps or Secrets require those resources to exist.
* GitHub Actions can be used as a pre-deployment validation step before manifests are applied to a cluster.

---

# Portfolio Value

This lab demonstrates junior DevOps troubleshooting skills by showing how to identify, explain, and fix common Kubernetes manifest issues.

It also demonstrates the ability to document technical problems clearly using a support-style format:

* Problem
* Impact
* Root Cause
* Fix
* Troubleshooting Lesson
