# Commands Used

## Purpose of This File

This file documents the commands and workflow steps used to build and validate this Kubernetes manifest troubleshooting lab.

The project does not deploy resources to a live Kubernetes cluster. Instead, it focuses on organizing broken and fixed Kubernetes manifests, documenting root causes, and validating the corrected manifests through GitHub Actions.

---

## Repository Setup

The repository was created for a junior-level Kubernetes troubleshooting and manifest validation lab.

```bash
mkdir kubernetes-manifest-validation-troubleshooting-lab
cd kubernetes-manifest-validation-troubleshooting-lab
git init
```

---

## Folder Structure Creation

The following folders were created to separate each troubleshooting scenario into broken and fixed manifest examples.

```bash
mkdir -p scenarios/01-service-selector-mismatch/broken
mkdir -p scenarios/01-service-selector-mismatch/fixed

mkdir -p scenarios/02-invalid-image-reference/broken
mkdir -p scenarios/02-invalid-image-reference/fixed

mkdir -p scenarios/03-port-mismatch/broken
mkdir -p scenarios/03-port-mismatch/fixed

mkdir -p scenarios/04-missing-configmap/broken
mkdir -p scenarios/04-missing-configmap/fixed

mkdir -p .github/workflows
mkdir -p notes
```

---

## File Creation

The following files were created for the README, GitHub Actions workflow, troubleshooting notes, and Kubernetes manifest scenarios.

```bash
touch README.md
touch .github/workflows/validate-k8s-manifests.yml
touch notes/commands-used.md
touch notes/troubleshooting-summary.md

touch scenarios/01-service-selector-mismatch/broken/deployment.yaml
touch scenarios/01-service-selector-mismatch/broken/service.yaml
touch scenarios/01-service-selector-mismatch/fixed/deployment.yaml
touch scenarios/01-service-selector-mismatch/fixed/service.yaml

touch scenarios/02-invalid-image-reference/broken/deployment.yaml
touch scenarios/02-invalid-image-reference/fixed/deployment.yaml

touch scenarios/03-port-mismatch/broken/deployment.yaml
touch scenarios/03-port-mismatch/broken/service.yaml
touch scenarios/03-port-mismatch/fixed/deployment.yaml
touch scenarios/03-port-mismatch/fixed/service.yaml

touch scenarios/04-missing-configmap/broken/deployment.yaml
touch scenarios/04-missing-configmap/fixed/configmap.yaml
touch scenarios/04-missing-configmap/fixed/deployment.yaml
```

---

## GitHub Actions Workflow File

The GitHub Actions workflow file is located at:

```text
.github/workflows/validate-k8s-manifests.yml
```

The workflow runs automatically on:

* Push to `main`
* Pull request to `main`

---

## YAML Syntax Validation

The workflow validates YAML syntax using Python and the PyYAML package.

The dependency is installed with:

```bash
python -m pip install pyyaml
```

The workflow then loads all YAML files under the `scenarios/` directory to confirm that the syntax is valid.

---

## Kubernetes Manifest Validation with kubeconform

This project originally attempted to validate manifests with `kubectl dry-run`, but that required access to a Kubernetes API server.

Because this project is designed to be clusterless, the workflow uses `kubeconform` instead.

`kubeconform` validates Kubernetes manifests against Kubernetes schemas without requiring a live Kubernetes cluster.

---

## kubeconform Installation in GitHub Actions

The workflow installs `kubeconform` with:

```bash
curl -L -o kubeconform.tar.gz https://github.com/yannh/kubeconform/releases/download/v0.6.7/kubeconform-linux-amd64.tar.gz
tar -xzf kubeconform.tar.gz
sudo mv kubeconform /usr/local/bin/kubeconform
kubeconform -v
```

---

## kubeconform Validation Commands

The workflow validates the fixed manifests with:

```bash
kubeconform -summary -strict scenarios/01-service-selector-mismatch/fixed/
kubeconform -summary -strict scenarios/02-invalid-image-reference/fixed/
kubeconform -summary -strict scenarios/03-port-mismatch/fixed/
kubeconform -summary -strict scenarios/04-missing-configmap/fixed/
```

---

## Git Commands

The following Git commands were used to add, commit, and push changes to GitHub.

```bash
git status
git add .
git commit -m "Add Kubernetes manifest validation troubleshooting lab"
git push origin main
```

Additional commits were used to fix and improve the project:

```bash
git add .github/workflows/validate-k8s-manifests.yml
git commit -m "Use kubeconform for clusterless manifest validation"
git push origin main
```

```bash
git add README.md
git commit -m "Update README for kubeconform validation workflow"
git push origin main
```

---

## Workflow Verification

After pushing changes to GitHub, the workflow can be verified from:

```text
GitHub Repository > Actions > Validate Kubernetes Manifests
```

A successful workflow confirms that:

* Required scenario directories exist
* YAML syntax validation passes
* `kubeconform` installs successfully
* Fixed Kubernetes manifests pass schema validation
* The repository is ready for review as a pre-deployment manifest validation lab

---

## Important Note

This project does not apply Kubernetes manifests to a live cluster.

The purpose is to demonstrate pre-deployment Kubernetes manifest troubleshooting and validation using GitHub Actions and `kubeconform`.
