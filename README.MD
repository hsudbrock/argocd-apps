The following need to be executed on the first time running the vault (in the `vault` namespace):
* `kubectl exec -it vault-app-0 -- vault operator init -n 1 -t 1`
* `kubectl exec -it vault-app-0 -- vault operator unseal <unseal-key>`
