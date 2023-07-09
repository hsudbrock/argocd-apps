The following need to be executed on the first time running the vault (in the `vault` namespace):
* `kubectl exec -it vault-app-0 -- vault operator init -n 1 -t 1`
* `kubectl exec -it vault-app-0 -- vault operator unseal <unseal-key>`

We also need to (manually) provide the external secrets operator a secret with the token for the vault, like this:
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: vault-token
  namespace: external-secrets-operator
data:
  token: <blablabla our token base64 encoded>
```

This can be generated using kubectl as follows:
```bash
kubectl create secret generic vault-token \
  --from-literal=token=test \
  --namespace=external-secrets-operator
```

Or updated as follows:
```bash
NEW_TOKEN=$(echo -n "new-token" | base64)

kubectl patch secret vault-token \
  --namespace=external-secrets-operator \
  --type='json' \
  -p='[{"op": "replace", "path": "/data/token", "value": "'$NEW_TOKEN'"}]'
 ```
