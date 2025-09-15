# Node.js App Deployment with Terraform & Ansible

Deploy a Node.js application on AWS EC2 using Terraform for infrastructure and Ansible for application deployment.

> **Note:** The Node.js application code is in a separate repository and will be automatically cloned during deployment.

## Prerequisites

- [Terraform](https://www.terraform.io/downloads) installed
- [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html) installed  
- AWS account with configured credentials
- AWS key pair (.pem file)
- Git installed

## Setup Instructions

### 1. Clone Repository
```bash
git clone https://github.com/<username>/nodejs-app-deployment.git
cd nodejs-app-deployment
```

### 2. Configure Ansible Inventory
Edit `ansible/inventory.ini`:
```ini
[ec2]
ubuntu@<PUBLIC_IP> ansible_ssh_private_key_file=/path/to/your/key.pem ansible_ssh_common_args='-o StrictHostKeyChecking=no'
```
**⚠️ Do NOT commit your .pem file to version control**

### 3. Deploy Infrastructure
```bash
cd terraform
terraform init
terraform plan
terraform apply
```
Type `yes` when prompted.

### 4. Deploy Application  
```bash
cd ../ansible
ansible-playbook -i inventory.ini playbook.yml
```

### 5. Access Application
Navigate to: `http://<PUBLIC_IP>:3007`

## Project Structure
```
nodejs-app-deployment/
├── terraform/           # Infrastructure code
├── ansible/            # Configuration & deployment
│   ├── inventory.ini   # Server details
│   └── playbook.yml    # Deployment tasks
└── README.md
```

## Configuration

### Custom Terraform Variables
Create `terraform/terraform.tfvars`:
```hcl
instance_type = "t2.micro"
key_name      = "your-key-pair-name"  
region        = "us-west-2"
```

## Troubleshooting

**SSH Connection Issues:**
- Check .pem file path in inventory.ini
- Set correct permissions: `chmod 400 your-key.pem`
- Verify EC2 instance is running

**Terraform Errors:**
- Confirm AWS credentials are configured
- Ensure key pair exists in specified region
- Check AWS permissions

**Application Not Loading:**
- Verify security group allows port 3007
- Check public IP address
- SSH to instance and verify app status

## Cleanup
```bash
cd terraform
terraform destroy
```

## Commands Reference

| Action | Command |
|--------|---------|
| Deploy infrastructure | `terraform apply` |
| Deploy application | `ansible-playbook -i inventory.ini playbook.yml` |
| Destroy infrastructure | `terraform destroy` |
| Debug Ansible | `ansible-playbook -i inventory.ini playbook.yml -vvv` |
