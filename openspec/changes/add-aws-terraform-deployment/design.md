# Design: AWS Terraform Deployment

## Architecture Overview

### High-Level Architecture
```
[Static Files] -> [S3 Bucket] -> [CloudFront Distribution] -> [Public URL]
     (Local)      (Storage)            (CDN)                  (End Users)
```

### Components

#### 1. S3 Bucket
- **Purpose**: Store static website files (HTML, CSS, images)
- **Configuration**:
  - Private bucket (not public S3 website hosting)
  - CloudFront Origin Access Identity (OAI) for secure access
  - Bucket policy allowing only CloudFront to read objects
  - No public access at bucket level

#### 2. CloudFront Distribution
- **Purpose**: CDN for fast global delivery
- **Configuration**:
  - S3 bucket as origin
  - Origin Access Identity for secure S3 access
  - Default root object: `index.html`
  - Price class: PriceClass_100 (North America & Europe - cost-effective)
  - Caching enabled with default TTL
  - HTTP to HTTPS redirect (viewer protocol policy)

#### 3. File Upload
- **Approach**: Use Terraform `aws_s3_object` resources to upload static files
- **Files to upload**:
  - `index.html` with `text/html` content type
  - `style.css` with `text/css` content type
  - `trashpanda.png` with `image/png` content type
- **Source**: Reference files from project root (`../` relative to terraform directory)

### Terraform Structure

```
terraform/
├── main.tf           # Primary configuration (provider, S3, CloudFront)
├── variables.tf      # Input variables (region, etc.)
├── outputs.tf        # Outputs (CloudFront URL, S3 bucket name)
├── terraform.tfvars  # Variable values (optional, gitignored)
└── .gitignore        # Ignore state files and variable files
```

### Key Design Decisions

#### Decision 1: Private S3 + OAI vs. Public S3 Website Hosting
- **Choice**: Private S3 with CloudFront OAI
- **Rationale**: 
  - More secure (bucket remains private)
  - Forces all traffic through CloudFront for better caching
  - Industry best practice
  - Prevents direct S3 access

#### Decision 2: Terraform File Upload vs. Manual Upload
- **Choice**: Terraform manages file uploads via `aws_s3_object`
- **Rationale**:
  - Complete infrastructure-as-code solution
  - Reproducible deployments
  - Simple for small static sites
  - Trade-off: Not ideal for frequent content updates (future: separate deployment pipeline)

#### Decision 3: State Management
- **Choice**: Local state file for initial implementation
- **Rationale**:
  - Simplest setup for single developer
  - Document in README with recommendation to upgrade to remote state (S3 + DynamoDB) for team environments
  - `.gitignore` prevents state file commits

#### Decision 4: CloudFront Cache Invalidation
- **Choice**: Manual invalidation process (documented)
- **Rationale**:
  - Terraform doesn't automatically invalidate on file changes
  - Document AWS CLI command for invalidation
  - Future enhancement: Add invalidation to CI/CD pipeline

### Security Considerations

1. **Bucket Access**:
   - Block all public access at bucket level
   - Use OAI for CloudFront-only access
   - Bucket policy explicitly allows only the OAI

2. **Sensitive Files**:
   - `.gitignore` includes:
     - `*.tfstate` and `*.tfstate.backup`
     - `terraform.tfvars` (may contain sensitive values)
     - `.terraform/` directory

3. **HTTPS**:
   - CloudFront configured to redirect HTTP to HTTPS
   - Viewer protocol policy enforces secure connections

### Deployment Workflow

1. **Initial Setup**:
   ```bash
   cd terraform
   terraform init
   terraform plan
   terraform apply
   ```

2. **Updates to Content**:
   - Modify HTML/CSS/images
   - Run `terraform apply` to sync files to S3
   - Invalidate CloudFront cache:
     ```bash
     aws cloudfront create-invalidation \
       --distribution-id <DISTRIBUTION_ID> \
       --paths "/*"
     ```

3. **Teardown**:
   ```bash
   terraform destroy
   ```

### Cost Estimation
- **S3**: ~$0.023 per GB/month (minimal for 3 files)
- **CloudFront**: Free tier includes 1TB data transfer out/month
- **Expected monthly cost**: < $1 for typical low-traffic static site

### Future Enhancements
- Remote state backend (S3 + DynamoDB)
- CI/CD pipeline (GitHub Actions)
- Custom domain + Route53
- ACM certificate for custom domain
- Multi-environment support (workspaces or separate configs)
- Automated CloudFront invalidation
- Web Application Firewall (WAF) integration
