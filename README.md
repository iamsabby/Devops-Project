# 🚀 Complete DevOps Workflow with Jenkins, Terraform & Ansible on Azure  
**Created by Sabih Ul Hassan**

This repository presents a full-fledged automation pipeline that provisions infrastructure on Microsoft Azure using **Terraform**, configures a web server through **Ansible**, and handles application deployment with **Jenkins** running inside a **Docker** container. The solution reflects a modern DevOps use case for deploying static web apps in the cloud.

---

## 🔗 Pipeline Flow Overview

```
Docker → Jenkins CI/CD → Terraform (Azure VM) → Ansible (Config) → Web App Hosted
```

---

## ⚙️ Stack Overview

- 🐳 **Docker** – Isolated environment for Jenkins
- 🧩 **Jenkins** – Orchestrates automation flow
- 🏗 **Terraform** – Defines and provisions Azure infrastructure
- 🔧 **Ansible** – Manages software configuration and app deployment
- ☁️ **Azure** – Cloud provider hosting the virtual machine
- 🔄 **Git** – Version control and source sync

---

## 📁 Project Layout

```
sabih-devops/
├── terraform/
│   ├── main.tf
│   ├── variables.tf
│   └── terraform.tfvars.example
├── ansible/
│   ├── install_web.yml
│   └── deploy_app.yml
├── app/
│   └── index.html
├── Jenkinsfile
└── README.md
```

---

## ✅ Requirements

- Azure account with valid subscription
- Docker installed on your local machine
- SSH public/private key pair

---

## 🐋 Step 1: Launch Jenkins via Docker

Run the following command to initialize a Jenkins container with pre-mounted tools:

```bash
docker run -d \
  --name jenkins \
  -p 8080:8080 -p 50000:50000 \
  -v jenkins_home:/var/jenkins_home \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v $(which terraform):/usr/local/bin/terraform \
  -v $(which ansible-playbook):/usr/local/bin/ansible-playbook \
  -v $(pwd):/workspace \
  jenkins/jenkins:lts
```

---

## 🛠 Step 2: Set Up Jenkins Dashboard

1. Visit `http://localhost:8080`  
2. Unlock Jenkins using the initial admin password  
3. Install suggested plugins and the following additional ones:
   - Git Plugin
   - Pipeline
   - SSH Agent

---

## 🔑 Step 3: Add Secret Credentials in Jenkins

Navigate to **Manage Jenkins → Credentials**, and add:

| ID                         | Type                     | Description                       |
|---------------------------|--------------------------|-----------------------------------|
| azure-subscription-id     | Secret Text              | Azure Subscription ID             |
| azure-client-id           | Secret Text              | Azure App (Client) ID             |
| azure-client-secret       | Secret Text              | Azure Client Secret               |
| azure-tenant-id           | Secret Text              | Azure Directory (Tenant) ID       |
| azure-vm-ssh-key          | SSH Private Key          | Private SSH key for VM login      |
| azure-vm-ssh-public-key   | Secret Text              | Corresponding public SSH key      |

---

## 📦 Step 4: Configure Infrastructure Settings

Copy and customize the example Terraform variables file:

```bash
cp terraform/terraform.tfvars.example terraform/terraform.tfvars
```

Edit the new `terraform.tfvars`:

```hcl
resource_group_name = "sabih-devops-rg"
location            = "East US"
prefix              = "sabih"
vm_size             = "Standard_B1s"
admin_username      = "azureuser"
ssh_public_key      = "ssh-rsa AAAAB3... sabih@example.com"
```

---

## 🧪 Step 5: Build Jenkins Pipeline

1. Create a new **Pipeline** job  
2. Link your Git repository containing this project  
3. Point to `Jenkinsfile` as the pipeline definition  
4. Run the job to execute your automated deployment

---

## 🚦 Pipeline Stages Overview

- **Code Checkout** – Grabs latest repo content  
- **Terraform Init & Apply** – Creates infrastructure  
- **Ansible Setup** – Installs and configures Apache web server  
- **App Deployment** – Pushes static HTML app  
- **Validation** – Confirms deployment success

---

## 🧰 Troubleshooting & Debugging

### Issues & Fixes

- **SSH Timeout**: 
  - Confirm NSG rules on Azure allow port 22  
  - Validate your key pair and access rights

- **Terraform Auth Errors**: 
  - Double-check your service principal credentials

- **Ansible Connection Fails**: 
  - Ensure VM is fully started before execution  
  - Use `-vvvv` for verbose Ansible logging

---

## 🧼 Cleanup After Deployment

To prevent unwanted charges, remove your resources:

```bash
cd terraform
terraform destroy -auto-approve
```

or via Azure CLI:

```bash
az group delete --name sabih-devops-rg --yes --no-wait
```

---

## 🔒 Best Practices

- Avoid committing secrets or `.tfvars` to your repository  
- Store secrets securely (e.g., Azure Key Vault)  
- Regularly rotate SSH keys and secrets  
- Apply strict NSG and firewall rules

---

## 📊 Future Enhancements

- ✅ Add automated testing pipeline steps  
- ✅ Set up remote state management  
- ✅ Integrate performance monitoring (e.g., Prometheus)  
- ✅ Enable alerting mechanisms  
- ✅ Improve cost visibility  
- ✅ Introduce secret managers

---

## 🤝 Contribution Steps

Want to contribute?

1. Fork this repository  
2. Create a feature branch  
3. Commit and push changes  
4. Submit a pull request

---

## 📜 License

This codebase is distributed under the **MIT License**.  
Check `LICENSE` for terms and conditions.

---

## 💬 Need Help?

- Recheck Jenkins job logs  
- Inspect Azure VM and NSG configurations  
- Review Terraform and Ansible output  
- Or raise an issue in the repo

---

### 🙌 Made with DevOps Passion by *Sabih Ul Hassan*
