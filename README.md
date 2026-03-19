# Cross-Platform-Azure-Updates-via-GitHub-Actions

## 📌 Project Overview
This project implements a professional, high-uptime automation suite using **GitHub Actions** and **Azure OpenID Connect (OIDC)**. It provides a "Remote Control" for system maintenance on both **Ubuntu 24.04 LTS** and **Windows Server** instances within the Azure Portal.

The goal is to eliminate manual SSH/RDP sessions and ensure all cloud resources are patched and audited via a centralized dashboard.

---

## 🏗 Architecture & Strategy
To ensure stability and follow a "Senior DevOps" approach, the project is split into two independent, manually-triggered workflows.

| Feature | Ubuntu Workflow | Windows Workflow |
| :--- | :--- | :--- |
| **Logic File** | `ubuntu-update.yml` | `windows-update.yml` |
| **Trigger** | Manual (`workflow_dispatch`) | Manual (`workflow_dispatch`) |
| **Package Manager**| `apt-get` | `Chocolatey (choco)` |
| **Auth Method** | Azure OIDC (Secretless) | Azure OIDC (Secretless) |
| **Execution** | One VM at a time | One VM at a time |

---

## 📂 Directory Structure
```text
.
├── .github/
│   └── workflows/
│       ├── ubuntu-update.yml    # Manual maintenance for Linux fleet
│       └── windows-update.yml   # Manual maintenance for Windows fleet
└── README.md                    # Project documentation
