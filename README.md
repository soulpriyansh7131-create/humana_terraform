# humana_terraform

Azure Terraform Infrastructure Repository with GitHub Actions CI/CD.

## 🚀 GitHub Actions CI/CD Workflow

This repository includes a multi-stage GitHub Actions workflow defined in [`.github/workflows/terraform.yml`](.github/workflows/terraform.yml).

### Workflow Triggers & Behavior

1. **Pull Requests (`pull_request`)**:
   - **Lint & Validate**: Runs `terraform fmt -check`, `terraform validate`, and `tflint`.
   - **Plan**: Runs `terraform init` & `terraform plan`, posting execution details to the GitHub Job Summary.

2. **Push to `main` / `master` (`push`)**:
   - Executes validation, plan generation, and automatically applies changes (`terraform apply`) to the target environment.

3. **Manual Trigger (`workflow_dispatch`)**:
   - Supports selecting the target **Environment** (`dev`, `qa`, `preprod`, `prod`).
   - Supports selecting the **Action**:
     - `plan`
     - `apply`
     - `destroy`

---

## 🔑 Required Repository Secrets

Configure the following GitHub Secrets under **Settings > Secrets and variables > Actions**:

| Secret Name | Description |
|---|---|
| `AZURE_CLIENT_ID` | Azure App Registration / User-Assigned Managed Identity Client ID |
| `AZURE_TENANT_ID` | Azure Active Directory (Microsoft Entra ID) Tenant ID |
| `AZURE_SUBSCRIPTION_ID` | Target Azure Subscription ID |

> **Note**: The workflow uses **Azure OpenID Connect (OIDC)** authentication (`ARM_USE_OIDC=true`). Ensure your Azure App Registration has a Federated Identity Credential pointing to your GitHub repository context.