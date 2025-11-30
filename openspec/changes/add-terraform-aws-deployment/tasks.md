# Tasks: Add Terraform AWS Deployment Infrastructure

## Phase 1: Terraform Configuration Files

1. **Create Terraform directory structure**
   - Create `terraform/` directory in project root
   - Verify directory structure matches design

2. **Create provider configuration** (`terraform/main.tf`)
   - Configure AWS provider
   - Set required provider version constraints
   - Add provider configuration block

3. **Create variables file** (`terraform/variables.tf`)
   - Define `aws_region` variable (default: `us-east-1`)
   - Define `bucket_name` variable (default: `simple-presence-{random}`)
   - Define `project_name` variable (default: `simple-presence`)
   - Add variable descriptions

4. **Create S3 bucket configuration** (`terraform/s3.tf`)
   - Define S3 bucket resource
   - Configure bucket settings (versioning, encryption)
   - Configure website configuration block
   - Add bucket policy for CloudFront OAI access

5. **Create CloudFront configuration** (`terraform/cloudfront.tf`)
   - Define Origin Access Identity resource
   - Define CloudFront distribution resource
   - Configure origin with S3 bucket and OAI
   - Configure default cache behavior
   - Set viewer protocol policy to redirect-to-https
   - Configure default root object

6. **Create outputs file** (`terraform/outputs.tf`)
   - Output S3 bucket name
   - Output S3 bucket website endpoint (if applicable)
   - Output CloudFront distribution ID
   - Output CloudFront distribution domain name
   - Add output descriptions

## Phase 2: Documentation

7. **Create Terraform README** (`terraform/README.md`)
   - Document prerequisites (AWS account, Terraform, AWS CLI)
   - Document required AWS permissions
   - Document deployment steps (`terraform init`, `terraform plan`, `terraform apply`)
   - Document how to upload files to S3
   - Document how to access the deployed site
   - Document cleanup steps (`terraform destroy`)

8. **Update main project README** (`README.md`)
   - Add deployment section referencing `terraform/README.md`
   - Note AWS requirements

## Phase 3: Validation

9. **Validate Terraform configuration**
   - Run `terraform init` to verify provider downloads
   - Run `terraform validate` to check syntax
   - Run `terraform fmt` to format files
   - Verify no linter errors

10. **Test Terraform plan** (dry-run)
    - Run `terraform plan` with test variables
    - Verify planned resources match design
    - Check for any unexpected resource creations

## Phase 4: File Organization

11. **Verify file structure**
    - Confirm all Terraform files are in `terraform/` directory
    - Verify no Terraform files in project root
    - Check that static files remain in root

## Dependencies
- Task 1 must complete before tasks 2-6
- Tasks 2-6 can be done in parallel after task 1
- Task 7 depends on tasks 2-6
- Task 8 depends on task 7
- Tasks 9-10 depend on tasks 2-6
- Task 11 depends on all previous tasks

## Validation Criteria
- ✅ `terraform validate` passes without errors
- ✅ `terraform plan` shows expected resources (S3 bucket, CloudFront distribution, OAI)
- ✅ All Terraform files follow HCL formatting standards
- ✅ Documentation is complete and accurate
- ✅ No Terraform files in project root (only in `terraform/` directory)

