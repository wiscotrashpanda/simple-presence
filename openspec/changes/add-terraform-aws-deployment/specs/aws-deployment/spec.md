# AWS Deployment Infrastructure Specification

## ADDED Requirements

### Requirement: Terraform Configuration Directory (REQ-AWS-001)
The project MUST include a `terraform/` directory containing all Terraform configuration files for AWS infrastructure deployment.

#### Scenario: Terraform Directory Structure
- **Given** the project root directory
- **When** a developer inspects the project structure
- **Then** a `terraform/` directory exists
- **And** all Terraform configuration files are located within `terraform/`
- **And** no Terraform files exist in the project root

---

### Requirement: AWS Provider Configuration (REQ-AWS-002)
Terraform MUST be configured to use the AWS provider with appropriate version constraints and region configuration.

#### Scenario: Provider Setup
- **Given** the Terraform configuration
- **When** `terraform init` is executed
- **Then** the AWS provider is downloaded and initialized
- **And** the provider version constraint is specified
- **And** the AWS region is configurable via variable (default: `us-east-1`)

---

### Requirement: S3 Bucket for Static Website Hosting (REQ-AWS-003)
An S3 bucket MUST be provisioned to store static website files (HTML, CSS, images).

#### Scenario: S3 Bucket Creation
- **Given** Terraform configuration is applied
- **When** `terraform apply` is executed
- **Then** an S3 bucket is created
- **And** the bucket name is configurable via variable
- **And** the bucket has website configuration with `index.html` as index document
- **And** the bucket is private (not publicly accessible)
- **And** server-side encryption is enabled

---

### Requirement: Origin Access Identity for S3 (REQ-AWS-004)
An Origin Access Identity (OAI) MUST be created to restrict S3 bucket access to CloudFront only.

#### Scenario: OAI Configuration
- **Given** Terraform configuration is applied
- **When** `terraform apply` is executed
- **Then** an Origin Access Identity is created
- **And** the OAI is associated with the CloudFront distribution
- **And** the S3 bucket policy allows access only from the OAI

---

### Requirement: CloudFront Distribution (REQ-AWS-005)
A CloudFront distribution MUST be provisioned to serve content from the S3 bucket via CDN.

#### Scenario: CloudFront Distribution Creation
- **Given** Terraform configuration is applied
- **When** `terraform apply` is executed
- **Then** a CloudFront distribution is created
- **And** the distribution uses the S3 bucket as origin
- **And** the distribution uses the OAI for origin access
- **And** the default root object is set to `index.html`
- **And** IPv6 is enabled
- **And** the viewer protocol policy redirects HTTP to HTTPS

#### Scenario: CloudFront Cache Behavior
- **Given** the CloudFront distribution is created
- **When** content is requested via CloudFront
- **Then** static assets (HTML, CSS, images) are cached appropriately
- **And** cache headers are set for optimal performance

---

### Requirement: Terraform Variables (REQ-AWS-006)
Terraform configuration MUST expose configurable variables for AWS region, bucket name, and project name.

#### Scenario: Variable Configuration
- **Given** the Terraform configuration
- **When** a developer reviews `terraform/variables.tf`
- **Then** `aws_region` variable is defined (default: `us-east-1`)
- **And** `bucket_name` variable is defined with a default pattern
- **And** `project_name` variable is defined (default: `simple-presence`)
- **And** all variables have descriptive comments

---

### Requirement: Terraform Outputs (REQ-AWS-007)
Terraform MUST output essential information about created resources (bucket name, CloudFront URL).

#### Scenario: Output Values
- **Given** Terraform configuration is applied
- **When** `terraform output` is executed
- **Then** S3 bucket name is displayed
- **And** CloudFront distribution ID is displayed
- **And** CloudFront distribution domain name (URL) is displayed
- **And** all outputs have descriptive descriptions

---

### Requirement: Deployment Documentation (REQ-AWS-008)
The `terraform/` directory MUST include a README.md file documenting the deployment process.

#### Scenario: Deployment Instructions
- **Given** a developer wants to deploy the infrastructure
- **When** they read `terraform/README.md`
- **Then** prerequisites are documented (AWS account, Terraform, AWS CLI)
- **And** required AWS permissions are listed
- **And** step-by-step deployment instructions are provided
- **And** instructions for uploading files to S3 are included
- **And** instructions for accessing the deployed site are included
- **And** cleanup instructions (`terraform destroy`) are provided

---

### Requirement: Terraform Configuration Validation (REQ-AWS-009)
All Terraform configuration files MUST pass validation and formatting checks.

#### Scenario: Configuration Validation
- **Given** Terraform configuration files are created
- **When** `terraform validate` is executed
- **Then** no validation errors are reported
- **And** `terraform fmt` reports no formatting issues
- **And** `terraform plan` executes without errors (assuming valid AWS credentials)

---

### Requirement: Project README Update (REQ-AWS-010)
The main project README MUST be updated to reference the deployment infrastructure.

#### Scenario: README Documentation
- **Given** the main project README
- **When** a developer reads the README
- **Then** a deployment section exists
- **And** the section references `terraform/README.md` for detailed instructions
- **And** AWS requirements are noted

