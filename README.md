# 🏗️ The Universal Architecture: Cross-Platform Multi-Cloud Patching

This project implements a professional, high-uptime automation suite using **GitHub Actions**, **Azure OIDC**, and **AWS OIDC**. It provides a centralized "Remote Control" for system maintenance on both Azure and AWS instances.

## 🚀 The Unified Workflow: `universal-patching.yml`
This workflow combines high-logic PowerShell and Bash scripts for Azure with native AWS SSM commands. It assumes two identities (one in Azure and one in AWS) using OpenID Connect (OIDC) to eliminate the need for long-lived credentials.

| Job | Platform | Target OS | Logic |
| :--- | :--- | :--- | :--- |
| **patch-azure** | Azure | Ubuntu & Windows | `az vm install-patches` + Custom PS |
| **patch-aws** | AWS | EC2 Fleet | `aws ssm send-command` (SSM Agent) |

---

## 🛠️ Configuration Checklist (AWS)
To ensure AWS "listens" to this script, perform these three steps:

### 1. Tag your EC2 Instances
AWS SSM uses tags to identify targets. Run this to tag your AWS VMs:
```bash
# Replace with your actual EC2 Instance IDs
aws ec2 create-tags --resources i-0xxxxxxxxxxxx i-0yyyyyyyyyyyy --tags Key=PatchGroup,Value=Production
```

### 2. Verify SSM Agent
Ensure the SSM Agent is "Online" on your instances:
```bash
aws ssm describe-instance-information --query "InstanceInformationList[*].{ID:InstanceId,Status:PingStatus,OS:PlatformName}" --output table
```

### 3. Establish AWS OIDC Trust
Add the AWS IAM Identity Provider for GitHub:
- **Provider URL**: `https://token.actions.githubusercontent.com`
- **Audience**: `sts.amazonaws.com`
- **Role Policy**: Ensure the role has `AmazonSSMFullAccess`.

---

## ✅ Validation
Once the workflow finishes, check the status of your global fleet:

### Azure Status
```bash
az vm get-instance-view -g Cross-Platform-Update -n Windows --query "instanceView.patchStatus"
```

### AWS Status
```bash
aws ssm list-command-invocations --details --query "CommandInvocations[*].{Instance:InstanceId,Status:Status}"
```

---

## 📂 Legacy Workflows
- `ubuntu-updates.yml`: Azure-only Ubuntu patching.
- `windows-updates.yml`: Azure-only Windows patching.
