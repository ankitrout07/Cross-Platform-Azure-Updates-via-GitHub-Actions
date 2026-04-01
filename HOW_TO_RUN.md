# 🚀 How to Run: Multi-Cloud Patching Project

Follow these steps to deploy and execute the automated patching suite for your global fleet.

## 📋 1. Prerequisites
Before running the workflows, ensure you have the following:

- **GitHub Secrets**: Add these to your repository:
  - `AZURE_CLIENT_ID`, `AZURE_TENANT_ID`, `AZURE_SUBSCRIPTION_ID`
  - `AWS_ROLE_ARN`
- **Cloud Configuration**:
  - **Azure**: OIDC configured for your subscription.
  - **AWS**: OIDC Trust Provider created and IAM Role with `AmazonSSMFullAccess`.
- **Fleet Tagging**:
  - AWS instances tagged with `OS=Ubuntu` or `OS=Windows`.
  - Azure instances named `Ubuntu` and `Windows` within the `Cross-Platform-Update` Resource Group.

---

## ⚡ 2. Triggering a Patch Run

### Option A: Automatic (CI/CD)
The project is built on the **Single Source of Truth** principle. Updating your patching logic automatically triggers a run:
1. Edit a script (e.g., `scripts/ubuntu/patch.sh`).
2. Commit and push the changes to the `main` branch.
3. GitHub Actions will detect the path change and start the corresponding workflow (`Ubuntu-MultiCloud-Patching`).

### Option B: Manual (On-Demand)
Use the "Big Green Button" for zero-day vulnerabilities:
1. Navigate to your GitHub repository's **Actions** tab.
2. Select either `Ubuntu-MultiCloud-Patching` or `Windows-MultiCloud-Patching`.
3. Click the **Run workflow** dropdown and select `Run workflow`.

### Option C: Targeted Cloud Selection (Selector Tool)
If you only need to patch one cloud provider (Azure or AWS) and a specific OS:
1. Navigate to your GitHub repository's **Actions** tab.
2. Select **🎯 Single-Cloud Patching Selector** in the left sidebar.
3. Click the **Run workflow** dropdown on the right.
4. **The Selector**:
   - Choose your **Cloud Provider** (AWS or Azure).
   - Choose your **OS Type** (Ubuntu or Windows).
5. Click the green **Run workflow** button.

---

## 📊 3. Monitoring & Verification

### In GitHub Actions
- Open the active workflow run to see live logs of script execution.
- Check both `Azure Patching` and `AWS Patching` steps for success.

### From your Terminal
Verify the status of your fleet using these commands:

**Azure Status**:
```bash
az vm get-instance-view -g Cross-Platform-Update -n Ubuntu --query "instanceView.patchStatus"
```

**AWS Status**:
```bash
aws ssm list-command-invocations --details --query "CommandInvocations[*].{Instance:InstanceId,Status:Status}"
```

---

## 🛠️ Troubleshooting
- **AWS SSM**: Ensure the SSM Agent is "Online" on your EC2 instances.
- **Azure OIDC**: Verify the Federated Credentials match your GitHub repo and branch.
- **Permissions**: Ensure the Cloud Roles have sufficient permissions (`VM Contributor` for Azure, `SSMFullAccess` for AWS).
