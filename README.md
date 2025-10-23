# APIMOps

Simple Azure DevOps pipelines for backing up and restoring Azure API Management configurations.

Inspired by the [Azure/apiops](https://github.com/Azure/apiops) project, this repository provides streamlined pipelines for:
- **Backing up** APIM configurations to version control
- **Restoring** APIM configurations from version control to target environments

## 🚀 Quick Start

### Prerequisites

1. **Azure DevOps** project with pipelines enabled
2. **Azure Service Connection** configured in Azure DevOps
3. **Azure API Management** instance(s)
4. **Permissions**: The service principal needs Contributor access to the APIM resource group

### Setup

1. **Clone this repository** to your Azure DevOps project
2. **Update configuration files** with your environment details:
   - `configuration.dev.yaml` - Development environment settings
   - `configuration.test.yaml` - Test environment settings  
   - `configuration.prod.yaml` - Production environment settings
3. **Create Azure DevOps pipelines**:
   - Import `pipelines/backup-apim.yml` for backup operations
   - Import `pipelines/restore-apim.yml` for restore operations
4. **Configure pipeline variables**:
   - Set `azureSubscription` to your Azure service connection name

## 📋 Pipelines

### Backup Pipeline (`backup-apim.yml`)

Extracts all APIM configurations and saves them to the repository.

**Parameters:**
- `apimServiceName` - Name of the APIM service to backup
- `resourceGroupName` - Resource group containing the APIM service
- `artifactsFolderPath` - Folder to store extracted configurations (default: 'artifacts')
- `commitMessage` - Custom commit message for the backup
- `createPullRequest` - Whether to create a PR or commit directly (default: true)

**Usage:**
1. Run the pipeline manually from Azure DevOps
2. Provide the required parameters
3. The pipeline will extract all configurations and create a PR (or commit directly)

### Restore Pipeline (`restore-apim.yml`)

Publishes APIM configurations from the repository to a target APIM service.

**Parameters:**
- `apimServiceName` - Name of the target APIM service
- `resourceGroupName` - Resource group of the target APIM service
- `artifactsFolderPath` - Folder containing the configurations to restore (default: 'artifacts')
- `environment` - Target environment (dev/test/staging/prod)
- `confirmRestore` - Safety confirmation for production deployments

**Features:**
- ✅ Environment-specific configuration overrides
- ✅ Production safety checks
- ✅ Deployment verification
- ✅ Rollback capabilities
- ✅ Detailed logging and reporting

## 📁 Repository Structure

```
APIMOps/
├── pipelines/
│   ├── backup-apim.yml          # Backup pipeline
│   └── restore-apim.yml         # Restore pipeline
├── artifacts/                   # Extracted APIM configurations
│   └── README.md               # Artifacts folder documentation
├── configuration.dev.yaml      # Development environment config
├── configuration.test.yaml     # Test environment config
├── configuration.prod.yaml     # Production environment config
├── configuration.extractor.yaml # Optional: Selective extraction config
└── README.md                   # This file
```

## ⚙️ Configuration

### Environment Configuration Files

Each environment has its own configuration file (`configuration.{env}.yaml`) that allows you to:

- Override APIM service names and resource groups
- Set environment-specific named values
- Configure backend service URLs for each environment
- Customize policies and settings per environment

Example configuration:
```yaml
apimServiceName: my-apim-prod
resourceGroupName: rg-apim-prod

namedValues:
  - name: environment
    value: prod
  - name: api-base-url
    value: https://api.mycompany.com

backends:
  - name: user-service
    url: https://user-api.mycompany.com
```

### Selective Extraction

Use `configuration.extractor.yaml` to extract only specific resources:

```yaml
apiNames:
  - critical-api-1
  - critical-api-2
  
productNames:
  - premium-product
  
# Only extract specified resources instead of everything
```

## 🔄 Typical Workflows

### 1. Regular Backup
```bash
# Schedule or run manually
Backup Pipeline → Extract configs → Create PR → Review & Merge
```

### 2. Environment Promotion
```bash
# Promote from dev to test/prod
Backup (source) → Review changes → Restore Pipeline (target environment)
```

### 3. Disaster Recovery
```bash
# Restore from last known good state
Restore Pipeline → Select artifacts → Deploy to new/recovered APIM
```

### 4. Configuration Drift Detection
```bash
# Compare current state with version control
Backup Pipeline → Compare with previous backup → Identify drift
```

## 🛡️ Security & Best Practices

### Production Safety
- ✅ Production deployments require explicit confirmation
- ✅ Deployment environments for approval workflows
- ✅ Verification steps after deployment
- ✅ Detailed audit logs

### Access Control
- ✅ Use Azure service principals with minimal required permissions
- ✅ Store secrets in Azure Key Vault (referenced in pipeline variables)
- ✅ Enable pipeline permissions and approvals for production

### Monitoring
- ✅ Pipeline execution logs
- ✅ APIM service health checks
- ✅ Configuration change notifications
- ✅ Deployment verification steps

## 🔧 Customization

### Adding New Environments

1. Create `configuration.{newenv}.yaml`
2. Update restore pipeline to include the new environment in parameters
3. Configure deployment environment in Azure DevOps if needed

### Custom Extraction Rules

Modify `configuration.extractor.yaml` to:
- Extract only specific APIs for team-based workflows
- Include/exclude certain resource types
- Filter by tags or other criteria

### Pipeline Modifications

The pipelines are designed to be simple and customizable:
- Add notification steps (Teams, email, etc.)
- Integrate with monitoring tools
- Add custom validation steps
- Include automated testing

## 📚 Additional Resources

- [Azure API Management Documentation](https://docs.microsoft.com/en-us/azure/api-management/)
- [Azure/apiops GitHub Repository](https://github.com/Azure/apiops)
- [Azure DevOps Pipelines Documentation](https://docs.microsoft.com/en-us/azure/devops/pipelines/)

## 🤝 Contributing

Feel free to customize these pipelines for your organization's needs. Common improvements:
- Add automated testing after deployment
- Integrate with monitoring and alerting systems
- Add support for additional environments
- Include rollback automation
- Add configuration validation steps

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.