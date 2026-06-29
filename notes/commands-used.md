# Commands Used

## Repository Setup

```bash
mkdir kubernetes-manifest-validation-troubleshooting-lab
cd kubernetes-manifest-validation-troubleshooting-lab
git init

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

kubectl create --dry-run=client --validate=false -f scenarios/01-service-selector-mismatch/fixed/ -o yaml

kubectl create --dry-run=client --validate=false -f scenarios/02-invalid-image-reference/fixed/ -o yaml

kubectl create --dry-run=client --validate=false -f scenarios/03-port-mismatch/fixed/ -o yaml

kubectl create --dry-run=client --validate=false -f scenarios/04-missing-configmap/fixed/ -o yaml

git status
git add .
git commit -m "Add Kubernetes manifest validation troubleshooting lab"
git branch -M main
git remote add origin https://github.com/michaeltucker24/kubernetes-manifest-validation-troubleshooting-lab.git
git push -u origin main