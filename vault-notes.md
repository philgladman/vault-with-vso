## Vault cli commands
- create secret - `vault kv put secret/test1 username=admin password=password123`
- list secrets in secret - `vault kv list secret`
- get new secret - `vault kv get secret/test1` or `vault kv get -mount=secret test1`
- get new secret outputed in json - `vault kv get -format=json secret/test1` or `vault kv get -mount=secret -format=json test1`
- get specific field username from secret - `vault kv get -field=username secret/test` or `vault kv get -mount=secret -field=username test1`

## Notes
- Continue to store all secrets in 

## Custom init.sh
- Vault Agent - side car on every pod like istio
- Vault CSI - Daemonset
- Vault Operator - only 1 Pod
- Deploy Vault dev from BB & run custom init script
- exec into pod and run `vault secrets enable kv-v2` to enable kv-v2 engine
- create test secret `vault kv put kv-v2/fakeapp/credentials username=admin password=password123`
- add policy to allow reading secret 
```
vault policy write mysecret - << EOF
    path "kv-v2/data/fakeapp/mysecret" {
    capabilities = ["read"]
    }
EOF
```

- enable k8s auth backend so VSO can authenticate with backend vault using k8s service accounts
```
vault auth enable kubernetes
```

- configure auth enging to talk to k8s api server
```
vault write auth/kubernetes/config kubernetes_host="https://$KUBERNETES_PORT_443_TCP_ADDR:443"
```

- create role to enable access to the secret
```
vault write auth/kubernetes/role/fakeapp \
  bound_service_account_names=default \
  bound_service_account_namespaces=fakeapp \
  policies=default,mysecret \
  audience=vault \
  ttl=24h
```

