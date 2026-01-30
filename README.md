``` bash
sops exec-env secrets.yaml 'echo $FLUX_GITHUB_PAT | flux bootstrap github --token-auth --private=false --owner=niule-eu --branch=main --personal --path=clusters/infra --repository=k0s-infra --components-extra=source-watcher'
```
