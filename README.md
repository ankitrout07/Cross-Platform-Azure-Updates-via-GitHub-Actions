# Cross-Platform-Azure-Updates-via-GitHub-Actions

## 📌 Project Overview
This project implements a professional, high-uptime automation suite using **GitHub Actions** and **Azure OpenID Connect (OIDC). It provides a "Remote Control" for system maintenance on both **Ubuntu 24.04 LTS** and **Windows Server** instances within the Azure Portal.

The goal is to eliminate manual SSH/RDP sessions and ensure all cloud resources are patched and audited via a centralized dashboard.

---

## 🏗 Architecture & Strategy
To ensure stability and follow a "Senior DevOps" approach, the project is split into two independent, manually-triggered workflows.

| Feature | Ubuntu Workflow | Windows Workflow |
| :--- | :--- | :--- |
| **Logic File** | `ubuntu-updates.yml` | `windows-updates.yml` |
| **Trigger** | Manual (`workflow_dispatch`) | Manual (`workflow_dispatch`) |
| **Package Manager**| `apt-get` | `Chocolatey (choco)` |
| **Auth Method** | Azure OIDC (Secretless) | Azure OIDC (Secretless) |
| **Execution** | One VM at a time | One VM at a time |

---
# Variables
RG="Cross-Platform-Update"

# 1. Trigger Ubuntu Patching
az vm install-patches -g $RG -n Ubuntu --reboot-setting IfRequired --classifications-to-include-linux Security Critical

# 2. Trigger Windows Patching
az vm install-patches -g $RG -n Windows --reboot-setting IfRequired --classifications-to-include-win Security Critical

# Variables
RG="Cross-Platform-Update"

# 1. Assess Windows
az vm assess-patches -g $RG -n Windows

# 2. Assess Ubuntu
az vm assess-patches -g $RG -n Ubuntu

az vm install-patches
