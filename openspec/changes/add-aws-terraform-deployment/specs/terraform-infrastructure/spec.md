# Terraform Infrastructure Specification

## ADDED Requirements

### Requirement: Terraform Configuration Structure
The project MUST include a `/terraform` directory at the project root containing all Terraform configuration files for AWS infrastructure.

#### Scenario: Developer initializes Terraform
**Given** the project repository is cloned  
**When** the developer navigates to the `/terraform` directory  
**Then** they find the following files:
- `main.tf` - primary infrastructure configuration
- `variables.tf` - input variable definitions
- `outputs.tf` - output value definitions
- `.gitignore` - excludes Terraform state and sensitive files

#### Scenario: Terraform state files are protected
**Given** a developer has run `terraform apply`  
**When** they check git status  
**Then** `terraform.tfstate`, `terraform.tfstate.backup`, `.terraform/` directory, and `terraform.tfvars` are ignored by git

---

### Requirement: S3 Bucket for Static Hosting
The Terraform configuration MUST create an S3 bucket to store static website files with appropriate security settings.

#### Scenario: S3 bucket is created with private access
**Given** the Terraform configuration is applied  
**When** the S3 bucket is created  
**Then** the bucket has:
- Public access blocked at all levels
- A unique bucket name
- Versioning disabled (optional for static sites)
- Server-side encryption enabled

#### Scenario: Bucket policy allows CloudFront access only
**Given** the CloudFront distribution has an Origin Access Identity  
**When** the bucket policy is applied  
**Then** only the CloudFront OAI can read objects from the bucket  
**And** direct public access to S3 URLs is denied

---

### Requirement: CloudFront Distribution for CDN
The Terraform configuration MUST create a CloudFront distribution to serve the static website globally with caching.

#### Scenario: CloudFront distribution is configured
**Given** the S3 bucket exists with static files  
**When** the CloudFront distribution is created  
**Then** it has:
- S3 bucket configured as the origin
- Origin Access Identity for secure S3 access
- Default root object set to `index.html`
- HTTP to HTTPS redirect enabled
- Caching enabled with default TTL

#### Scenario: CloudFront URL is accessible
**Given** the CloudFront distribution is deployed  
**When** a user navigates to the CloudFront domain URL  
**Then** the website loads successfully  
**And** all assets (HTML, CSS, images) are served correctly

---

### Requirement: Static File Upload via Terraform
The Terraform configuration MUST upload the static website files (HTML, CSS, images) to the S3 bucket with correct content types.

#### Scenario: HTML file is uploaded with correct content type
**Given** the `index.html` file exists in the project root  
**When** Terraform applies the configuration  
**Then** the file is uploaded to the S3 bucket  
**And** the content type is set to `text/html`

#### Scenario: CSS file is uploaded with correct content type
**Given** the `style.css` file exists in the project root  
**When** Terraform applies the configuration  
**Then** the file is uploaded to the S3 bucket  
**And** the content type is set to `text/css`

#### Scenario: Image file is uploaded with correct content type
**Given** the `trashpanda.png` file exists in the project root  
**When** Terraform applies the configuration  
**Then** the file is uploaded to the S3 bucket  
**And** the content type is set to `image/png`

---

### Requirement: Terraform Variables
The Terraform configuration MUST define configurable variables for flexible deployment across different environments.

#### Scenario: AWS region is configurable
**Given** the `variables.tf` file defines an `aws_region` variable  
**When** a user applies Terraform without specifying the region  
**Then** it defaults to `us-east-1`  
**And** the user can override it via command line or `terraform.tfvars`

#### Scenario: Project name is configurable
**Given** the `variables.tf` file defines a `project_name` variable  
**When** resources are created  
**Then** they include the project name in resource names/tags for identification

---

### Requirement: Terraform Outputs
The Terraform configuration MUST output essential information needed to access and manage the deployed infrastructure.

#### Scenario: CloudFront URL is outputted
**Given** the Terraform apply completes successfully  
**When** the user views Terraform outputs  
**Then** the CloudFront distribution domain name is displayed  
**And** it is labeled as `cloudfront_url` or similar

#### Scenario: S3 bucket name is outputted
**Given** the Terraform apply completes successfully  
**When** the user views Terraform outputs  
**Then** the S3 bucket name is displayed  
**And** it can be used for manual file uploads or debugging

#### Scenario: CloudFront distribution ID is outputted
**Given** the Terraform apply completes successfully  
**When** the user views Terraform outputs  
**Then** the CloudFront distribution ID is displayed  
**And** it can be used for cache invalidation commands

---

### Requirement: AWS Provider Configuration
The Terraform configuration MUST configure the AWS provider with appropriate region settings.

#### Scenario: AWS provider is configured
**Given** the `main.tf` file contains a provider block  
**When** Terraform initializes  
**Then** the AWS provider is configured with the specified region variable  
**And** credentials are sourced from standard AWS credential chain (environment variables, AWS CLI config, or IAM roles)
