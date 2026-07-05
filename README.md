# aws-infra-core
aws-infra-core/
├── .github/
│   └── workflows/                # Automatic syntax linting for TF & Ansible on Pull Requests
├── terraform/                    # --- SPRINT 2: Infrastructure as Code ---
│   ├── main.tf                   # Root configuration calling network and compute modules
│   ├── providers.tf              # Configures AWS provider and S3/DynamoDB remote state backend
│   ├── variables.tf              # Global environment inputs (CIDR blocks, region, env name)
│   ├── outputs.tf                # Exposes VPC IDs and EKS cluster endpoints for external usage
│   ├── terraform.tfvars          # Environment-specific configuration variables (dev/prod values)
│   └── modules/                  # Reusable infrastructure blocks
│       ├── vpc/
│       │   ├── main.tf           # Allocates 3 Public and 3 Private subnets, IGW, and NAT Gateways
│       │   ├── variables.tf
│       │   └── outputs.tf
│       └── eks/
│           ├── main.tf           # Provisions EKS Control Plane, OIDC, and Private Managed Node Groups
│           ├── variables.tf
│           └── outputs.tf
├── ansible/                      # --- SPRINT 3: Configuration Management ---
│   ├── ansible.cfg               # Enables the aws_ec2 dynamic inventory plugin and SSH defaults
│   ├── inventory/
│   │   └── aws_ec2.yml           # Dynamic inventory configuration filtering nodes by AWS tags
│   ├── playbooks/
│   │   └── site.yml              # Master playbook orchestrating host provisioning
│   └── roles/
│       └── node_hardener/        # Modular configuration tasks
│           ├── tasks/
│           │   └── main.yml      # OS updates, firewall setups, Docker engine, and kubectl installation
│           └── templates/
│               └── sysctl.conf.j2 # Linux kernel tuning optimization templates
├── k8s-cluster-init/             # --- SPRINT 4 & 5: System Cluster Add-ons ---
│   ├── apps/
│   │   ├── aws-load-balancer-controller/
│   │   │   └── values.yaml       # Helm configuration overrides for the native AWS ALB engine
│   │   └── kube-prometheus-stack/
│   │       ├── values.yaml       # Helm configuration for Prometheus scraping metrics & Grafana
│   │       ├── alerting-rules.yaml # Task 5.3: Core alerts (PodFrequentlyRestarting, DiskLow)
│   │       └── alertmanager.yml  # Task 5.4: Webhook routing integration matrix (e.g., Slack)
│   └── Makefile                  # Direct terminal shortcuts for initial cluster bootstrapping
├── Jenkinsfile.infra             # Task 2.4: Jenkins Declarative Pipeline (Init -> Validate -> Plan -> Approval -> Apply)
└── RUNBOOK.md                    # Task 6.3: Master DevOps execution runbook and architecture guide
