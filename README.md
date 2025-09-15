# Node.js App Deployment with Terraform & Ansible

This repository contains Terraform and Ansible code to deploy a Node.js application on an AWS EC2 instance.

## Project Structure

nodejs-app-deployment/
├── terraform/
│ └── main.tf # Terraform script to create EC2 instance & security group
├── ansible/
│ ├── inventory.ini # Ansible inventory file
│ └── playbook.yml # Ansible playbook to deploy Node.js app
└── .gitignore # Ignore sensitive and unnecessary files

> **Note:** The Node.js application itself is in a separate GitHub repository. Ansible will automatically clone it during deployment.

---

## Prerequisites

- [Terraform](https://www.terraform.io/downloads)
- [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)
- AWS account with a key pair (PEM file)
- Git installed

---

## Setup Instructions

1. **Clone this repository**:

```bash
git clone https://github.com/<username>/nodejs-app-deployment.git
cd nodejs-app-deployment
Configure AWS key in inventory.ini:

[ec2]
ubuntu@<PUBLIC_IP> ansible_ssh_private_key_file=/path/to/your/key.pem

Do NOT commit your PEM file. Keep it locally.

Deploy EC2 instance with Terraform:

cd terraform
terraform init
terraform plan
terraform apply

To destroy the instance:

terraform destroy

Deploy Node.js app with Ansible:

cd ../ansible
ansible-playbook -i inventory.ini playbook.yml

Verify the app:

Open your browser and navigate to http://<PUBLIC_IP>:3007
You should see your Node.js application running.