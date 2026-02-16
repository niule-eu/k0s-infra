``` bash
sops exec-env secrets.yaml 'echo $FLUX_GITHUB_PAT | flux bootstrap github --token-auth --private=false --owner=niule-eu --branch=main --personal --path=clusters/infra --repository=k0s-infra --components-extra=source-watcher'
```

## Argo Workflows Deployment Status

✅ **Completed** - Argo Workflows configuration for resource-constrained radxax4 node (N100 CPU, 8GB RAM):

- ✅ Created Argo Workflows HelmRepository resource
- ✅ Created Argo Workflows Namespace resource  
- ✅ Created Argo Workflows HelmRelease with resource constraints
- ✅ Added argo-workflows.yaml to infrastructure controllers kustomization
- ✅ Generated argo-workflows.yaml file
- ✅ Tested PKL rendering
- ✅ Optimized configuration by disabling unnecessary features

**Next Steps:**
- [ ] Test Argo Workflows deployment on radxax4 node
- [ ] Monitor resource usage and adjust limits as needed

**Resource constraints for 8GB node:**
- Workflow Controller: 500m CPU / 1Gi RAM
- Workflow Server: **Disabled** (saves ~512Mi RAM)  
- Executor: 100m CPU / 256Mi RAM
- **Total Argo footprint**: ~1.25Gi RAM (optimized for constrained environment)

**Optimizations Applied:**
- Server component disabled (UI/CLI not critical for homelab)
- Telemetry and metrics disabled
- Parallelism reduced to 1 workflow
- TTL reduced to 12 hours for aggressive cleanup
- Persistence and artifact collection disabled
- Minimal service accounts with separate permissions

**Deployment Files:**
- `infrastructure/controllers/argo-workflows.pkl` - PKL configuration
- `infrastructure/controllers/argo-workflows.yaml` - Generated YAML manifest
