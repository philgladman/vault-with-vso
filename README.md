# vault-with-vso
Test pod to use secrets stored in Vault by using Vault Secrets Operator

## Dev mode enabled
- Deploy Vault to kubernetes cluster with init script
- Deploy Vault Secrets Operator to cluster
- Deploy test pod to use secret in Vault via Vault Secrets Operator
- Create secret - `vault kv put kv-v2/fakeapp/credentials username=admin password=password123`
- Confirm pod has access to secret

## Dev mode disabled
- Deploy Vault to kubernetes cluster 
- Exec into vault-0 pod and run `vault operator init`
- Copy 5 keys and root token
- Unseal Vault by running `vault operator unseal` and paste in 3 of the 5 keys
- Pod should be in ready state now
- Log into vault with root token `vault login`
- Run init script
- Create secret - `vault kv put kv-v2/fakeapp/credentials username=admin password=password123`
- Deploy Vault Secrets Operator to cluster
- Deploy test pod
- Confirm pod has access to secret


# MISC
- Create helm release for Vault
    - `cd` into repo
    - Then run `helm template vault charts/vault/ -f kustomization/vault/vault-values-overrides.yaml -n vault --include-crds --debug > kustomization/vault/vault-helm-release.yaml`
- Create helm release for Vault Secrets Operator
    - `cd` into repo
    - Then run `helm template vault-secrets-operator charts/vault-secrets-operator -f kustomization/vault-secrets-operator/vso-values-overrides.yaml -n vault --include-crds --debug > kustomization/vault-secrets-operator/vso-helm-release.yaml`