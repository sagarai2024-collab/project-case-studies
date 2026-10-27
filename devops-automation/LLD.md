# DevOps & Automation — Low-Level Design (LLD)

Author: Sagarika Chakraborty — Full Stack .NET Engineer | React.js | Web API | SQL Server

## 1. Pipeline Stages
- Validate: PR checks, semantic versioning, commit lint.
- Build: restore, build, unit tests, code coverage.
- Package: publish artifacts, push to Azure Artifacts (nuget/npm).
- Deploy Staging: infra validation, slot deploy, config transform.
- Verify Staging: smoke tests, health checks, synthetic probes.
- Approval: Azure AD PIM approval gate for production.
- Deploy Prod: slot swap to production, warmup, post-deploy tests.
- Observe: App Insights dashboards, alerts; autoscale rules.

## 2. YAML Skeleton
```yaml
trigger:
  branches: { include: [ main ] }

stages:
- stage: Build_Test
  jobs:
  - job: Build
    pool: { vmImage: 'windows-latest' }
    steps:
    - task: UseDotNet@2
      inputs: { packageType: 'sdk', version: '8.x' }
    - script: dotnet restore
    - script: dotnet build --configuration Release
    - script: dotnet test --configuration Release --collect:"XPlat Code Coverage"
    - task: PublishBuildArtifacts@1
      inputs: { PathtoPublish: '$(Build.ArtifactStagingDirectory)', ArtifactName: 'drop' }

- stage: Deploy_Staging
  dependsOn: Build_Test
  jobs:
  - deployment: Staging
    environment: 'staging'
    strategy:
      runOnce:
        deploy:
          steps:
          - task: AzureWebApp@1
            inputs:
              appType: 'webApp'
              azureSubscription: '$(AZURE_CONN)'
              appName: '$(API_APP_STG)'
              package: '$(Pipeline.Workspace)/drop/**/*.zip'
          - task: AzureFunctionApp@2
            inputs:
              azureSubscription: '$(AZURE_CONN)'
              appType: 'functionApp'
              appName: '$(FUNC_APP_STG)'
              package: '$(Pipeline.Workspace)/drop/**/*.zip'
          - script: |
              pwsh ./scripts/smoke-tests.ps1 -BaseUrl $(STAGING_URL)
            displayName: Smoke Tests

- stage: PIM_Approval
  dependsOn: Deploy_Staging
  approval: # conceptually represented; use environment approvals with PIM
    - name: 'Prod Approval via PIM'

- stage: Deploy_Prod
  dependsOn: PIM_Approval
  jobs:
  - deployment: ProdSwap
    environment: 'production'
    strategy:
      runOnce:
        deploy:
          steps:
          - task: AzureAppServiceManage@0
            inputs:
              azureSubscription: '$(AZURE_CONN)'
              Action: 'Swap Slots'
              WebAppName: '$(API_APP)'
              SourceSlot: 'staging'
              DestinationSlot: 'production'
          - task: AzureAppServiceManage@0
            inputs:
              azureSubscription: '$(AZURE_CONN)'
              Action: 'Swap Slots'
              WebAppName: '$(FUNC_APP)'
              SourceSlot: 'staging'
              DestinationSlot: 'production'
          - script: pwsh ./scripts/post-deploy.ps1
            displayName: Post-deploy
```

## 3. PowerShell Automation
- `scripts/bootstrap.ps1` — create RGs, App Services, Function Apps, Key Vault secrets.
- `scripts/smoke-tests.ps1` — ping health endpoints; fail fast on non-200.
- `scripts/post-deploy.ps1` — purge CDN/cache, rotate keys if required, notify channels.

## 4. Proxy Integration
- App settings: outbound proxy URL and allowlist.
- Health probe exemptions and retries via Polly where used.
- Audit and change control for proxy exception list.

## 5. Secrets and Identity
- Use Key Vault with Managed Identity.
- No secrets in YAML; use variable groups linked to Key Vault.
- PIM roles required for prod release approvals.

## 6. Observability
- App Insights SDK enabled; distributed tracing.
- Alert rules: availability, failure rate, latency thresholds.
- Dashboards for deployment success, lead time, and MTTR.

## 7. Rollback and DR
- Swap back to revert instantly.
- Keep N previous artifacts; include DB migration down-scripts if needed.
- Document runbooks for failed swaps, hotfix path, and incident comms.

## 8. Artifacts and Versioning
- Semantic versioning; artifact retention policy.
- Internal packages published to Azure Artifacts feed.
- SBOM generation recommended (e.g., CycloneDX).

## 9. Governance
- Branch protection, PR templates, mandatory reviews.
- Environment-specific approvals; least-privilege service connections.
- Change logs and release notes published automatically.
