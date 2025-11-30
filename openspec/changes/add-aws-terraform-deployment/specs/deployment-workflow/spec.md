# Deployment Workflow Specification

## ADDED Requirements

### Requirement: Terraform README Documentation
The `/terraform` directory MUST include a README.md file with clear instructions for deploying and managing the infrastructure.

#### Scenario: README includes prerequisites
**Given** a developer opens the terraform README  
**When** they read the prerequisites section  
**Then** they find:
- AWS account requirement
- Terraform CLI installation instructions
- AWS credentials configuration guidance

#### Scenario: README includes deployment steps
**Given** a developer wants to deploy the site  
**When** they follow the README deployment section  
**Then** they find step-by-step commands:
- `terraform init` to initialize
- `terraform plan` to preview changes
- `terraform apply` to create infrastructure

#### Scenario: README includes update workflow
**Given** a developer updates the website content  
**When** they consult the README  
**Then** they find instructions for:
- Running `terraform apply` to sync changes
- Creating CloudFront cache invalidation
- AWS CLI command for invalidation

#### Scenario: README includes teardown instructions
**Given** a developer wants to remove the infrastructure  
**When** they consult the README  
**Then** they find the `terraform destroy` command and warnings about data loss

---

### Requirement: Git Ignore Configuration
The Terraform directory MUST include a `.gitignore` file to prevent committing sensitive and generated files.

#### Scenario: Terraform state files are ignored
**Given** the `.gitignore` file exists in `/terraform`  
**When** Terraform state files are generated  
**Then** the following patterns are ignored:
- `*.tfstate`
- `*.tfstate.backup`
- `*.tfstate.lock.info`

#### Scenario: Terraform working directory is ignored
**Given** the `.gitignore` file exists  
**When** `terraform init` creates the `.terraform/` directory  
**Then** the entire `.terraform/` directory is ignored by git

#### Scenario: Variable files with secrets are ignored
**Given** the `.gitignore` file exists  
**When** a developer creates `terraform.tfvars` with sensitive values  
**Then** the file is ignored by git  
**And** an example file `terraform.tfvars.example` can be committed

---

### Requirement: Initial Deployment Process
The system MUST support a straightforward initial deployment process from a clean state.

#### Scenario: First-time deployment succeeds
**Given** AWS credentials are configured  
**And** Terraform CLI is installed  
**And** the user is in the `/terraform` directory  
**When** the user runs:
```bash
terraform init
terraform plan
terraform apply
```
**Then** the S3 bucket is created  
**And** static files are uploaded to S3  
**And** CloudFront distribution is created  
**And** the CloudFront URL is displayed in outputs  
**And** navigating to the URL shows the website

---

### Requirement: Content Update Process
The system MUST support updating website content and redeploying changes.

#### Scenario: Content changes are synced to S3
**Given** the infrastructure is already deployed  
**When** a developer modifies `index.html`, `style.css`, or `trashpanda.png`  
**And** runs `terraform apply`  
**Then** the updated files are uploaded to S3  
**And** the file content in S3 matches the local files

#### Scenario: CloudFront cache can be invalidated
**Given** new content is uploaded to S3  
**When** the developer runs the CloudFront invalidation command:
```bash
aws cloudfront create-invalidation \
  --distribution-id <DISTRIBUTION_ID> \
  --paths "/*"
```
**Then** the CloudFront cache is cleared  
**And** the website reflects the updated content immediately

---

### Requirement: Infrastructure Teardown Process
The system MUST support complete removal of all AWS resources created by Terraform.

#### Scenario: Infrastructure is destroyed cleanly
**Given** the infrastructure is deployed  
**When** the user runs `terraform destroy` and confirms  
**Then** all AWS resources are deleted:
- S3 bucket (including all objects)
- CloudFront distribution
- Origin Access Identity  
**And** no AWS resources remain that incur costs

#### Scenario: Terraform state reflects destruction
**Given** `terraform destroy` completed successfully  
**When** the user runs `terraform show`  
**Then** the state shows no resources are managed

---

### Requirement: Cost Visibility
The documentation MUST provide visibility into expected AWS costs for running the infrastructure.

#### Scenario: Cost estimates are documented
**Given** a user reads the terraform README  
**When** they review the cost section  
**Then** they find estimated costs for:
- S3 storage (per GB/month)
- CloudFront data transfer (Free Tier mentioned)
- Typical monthly cost for low-traffic site

#### Scenario: Free tier eligibility is documented
**Given** a user is considering deployment costs  
**When** they read the cost documentation  
**Then** they find information about:
- CloudFront Free Tier (1TB data transfer/month)
- AWS Free Tier eligibility for new accounts
