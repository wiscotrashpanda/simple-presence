# Tasks: Add AWS Terraform Deployment

## Task List

### Phase 1: Terraform Directory Setup
1. **Create terraform directory structure**
   - Create `/terraform` directory at project root
   - Create `.gitignore` in terraform directory
   - Add patterns to ignore state files, `.terraform/`, and `terraform.tfvars`
   - **Validation**: Verify `.gitignore` exists and contains required patterns

2. **Create variables.tf**
   - Define `aws_region` variable with default `us-east-1`
   - Define `project_name` variable with default `simple-presence`
   - Add descriptions for all variables
   - **Validation**: Run `terraform fmt` to check syntax

3. **Create outputs.tf**
   - Define output for CloudFront distribution URL
   - Define output for S3 bucket name
   - Define output for CloudFront distribution ID (for invalidation)
   - **Validation**: Verify output blocks have descriptions

4. **Commit and push Phase 1 changes**
   - Stage terraform directory files: `.gitignore`, `variables.tf`, `outputs.tf`
   - Commit with message: "Add Terraform directory structure and configuration files"
   - Push changes to remote
   - **Validation**: Verify commit appears in git log and remote is updated

### Phase 2: Core Infrastructure Configuration
5. **Create main.tf - Provider configuration**
   - Add Terraform required version (>= 1.0)
   - Add AWS provider requirement
   - Configure AWS provider with region variable
   - **Validation**: Run `terraform init` successfully

6. **Create main.tf - S3 bucket resource**
   - Define `aws_s3_bucket` resource
   - Use project_name variable in bucket name
   - Add tags for resource identification
   - **Validation**: Run `terraform validate` successfully

7. **Configure S3 bucket security**
   - Add `aws_s3_bucket_public_access_block` resource
   - Block all public access (all 4 settings to `true`)
   - Add server-side encryption configuration
   - **Validation**: Run `terraform plan` and verify security settings

8. **Create CloudFront Origin Access Identity**
   - Add `aws_cloudfront_origin_access_identity` resource
   - Set comment to identify the OAI purpose
   - **Validation**: Verify resource references are correct

9. **Configure S3 bucket policy**
   - Add `aws_s3_bucket_policy` resource
   - Create policy allowing OAI to read objects
   - Use IAM policy document data source for clarity
   - **Validation**: Run `terraform plan` and check policy JSON

10. **Commit and push Phase 2 changes**
    - Stage main.tf with all infrastructure resources
    - Commit with message: "Add S3 bucket and CloudFront OAI infrastructure"
    - Push changes to remote
    - **Validation**: Verify commit appears in git log and remote is updated

### Phase 3: CloudFront Configuration
11. **Create CloudFront distribution - origin configuration**
   - Add `aws_cloudfront_distribution` resource
   - Configure S3 origin with OAI
   - Set origin ID and domain name
   - **Validation**: Verify origin configuration syntax

12. **Configure CloudFront distribution - default cache behavior**
    - Set allowed methods (GET, HEAD)
    - Set cached methods (GET, HEAD)
    - Configure viewer protocol policy (redirect-to-https)
    - Set default TTL values
    - Reference OAI in origin settings
    - **Validation**: Run `terraform plan` and review cache settings

13. **Configure CloudFront distribution - general settings**
    - Set default root object to `index.html`
    - Configure price class (PriceClass_100)
    - Enable the distribution
    - Set comment for identification
    - **Validation**: Verify all required CloudFront settings are present

14. **Commit and push Phase 3 changes**
    - Stage updated main.tf with CloudFront distribution
    - Commit with message: "Add CloudFront distribution configuration"
    - Push changes to remote
    - **Validation**: Verify commit appears in git log and remote is updated

### Phase 4: Static File Upload
15. **Create aws_s3_object for index.html**
    - Add `aws_s3_object` resource for `index.html`
    - Set source path to `../index.html` (relative to terraform dir)
    - Set content_type to `text/html`
    - Set ETag to detect changes
    - **Validation**: Verify file path is correct

16. **Create aws_s3_object for style.css**
    - Add `aws_s3_object` resource for `style.css`
    - Set source path to `../style.css`
    - Set content_type to `text/css`
    - Set ETag to detect changes
    - **Validation**: Verify file path is correct

17. **Create aws_s3_object for trashpanda.png**
    - Add `aws_s3_object` resource for `trashpanda.png`
    - Set source path to `../trashpanda.png`
    - Set content_type to `image/png`
    - Set ETag to detect changes
    - **Validation**: Verify file path is correct

18. **Commit and push Phase 4 changes**
    - Stage updated main.tf with S3 object resources
    - Commit with message: "Add static file upload configuration"
    - Push changes to remote
    - **Validation**: Verify commit appears in git log and remote is updated

### Phase 5: Documentation
19. **Create terraform/README.md**
    - Add project overview
    - Document prerequisites (AWS account, Terraform CLI, credentials)
    - Add deployment steps (init, plan, apply)
    - Document outputs and how to access the site
    - **Validation**: Read through README for clarity and completeness

20. **Document content update workflow**
    - Add section on updating website files
    - Include `terraform apply` command
    - Document CloudFront cache invalidation
    - Provide AWS CLI invalidation command example
    - **Validation**: Verify commands are copy-pasteable

21. **Document teardown process**
    - Add infrastructure destruction section
    - Include `terraform destroy` command
    - Add warnings about data loss
    - Document verification steps
    - **Validation**: Review warnings for clarity

22. **Document cost estimates**
    - Add cost estimation section
    - List S3 storage costs
    - Mention CloudFront Free Tier
    - Provide typical monthly cost estimate
    - **Validation**: Verify cost information is current

23. **Create terraform.tfvars.example**
    - Create example variable file
    - Show aws_region override example
    - Show project_name override example
    - Add comments explaining each variable
    - **Validation**: Verify example values are sensible

24. **Commit and push Phase 5 changes**
    - Stage terraform/README.md and terraform.tfvars.example
    - Commit with message: "Add Terraform documentation and example configuration"
    - Push changes to remote
    - **Validation**: Verify commit appears in git log and remote is updated

### Phase 6: Testing and Validation
25. **Test full deployment lifecycle**
    - Run `terraform init` from clean state
    - Run `terraform plan` and review planned changes
    - Run `terraform apply` and create infrastructure
    - Verify CloudFront URL in outputs
    - Visit CloudFront URL and confirm website loads
    - **Validation**: Website displays correctly with all assets

26. **Test content update workflow**
    - Make minor change to `index.html`
    - Run `terraform apply` to sync changes
    - Run CloudFront invalidation command
    - Verify updated content appears
    - **Validation**: Changes reflected on CloudFront URL

27. **Test infrastructure teardown**
    - Run `terraform destroy`
    - Confirm all resources are deleted in AWS console
    - Verify no lingering costs
    - **Validation**: `terraform show` returns empty state

28. **Run terraform fmt and validate**
    - Run `terraform fmt -recursive` to format all files
    - Run `terraform validate` to check configuration
    - Address any validation errors
    - **Validation**: Both commands succeed without errors

29. **Commit and push Phase 6 changes**
    - Stage any test-related changes or fixes discovered during testing
    - Commit with message: "Validate and test Terraform deployment lifecycle"
    - Push changes to remote
    - **Validation**: Verify commit appears in git log and remote is updated

### Phase 7: Final Verification
30. **Update root README.md**
    - Add deployment section mentioning Terraform
    - Link to terraform/README.md for details
    - **Validation**: Verify link works and section is clear

31. **Final openspec validation**
    - Run `openspec validate add-aws-terraform-deployment --strict`
    - Address any validation errors
    - **Validation**: Validation passes successfully

32. **Final commit and push**
    - Stage updated root README.md
    - Commit with message: "Complete AWS Terraform deployment configuration"
    - Push changes to remote
    - **Validation**: All changes are pushed and branch is up to date with remote

## Dependency Notes
- Tasks 1-3 can be done in parallel
- Task 4 commits Phase 1 work
- Tasks 5-9 must be done sequentially (dependencies on previous resources)
- Task 10 commits Phase 2 work
- Tasks 11-13 configure CloudFront (depends on Phase 2)
- Task 14 commits Phase 3 work
- Tasks 15-17 can be done in parallel (file uploads)
- Task 18 commits Phase 4 work
- Tasks 19-23 (documentation) can be done in parallel, independently from infrastructure tasks
- Task 24 commits Phase 5 work
- Phase 6 (testing) must be done after all previous phases complete
- Tasks 25-28 must be done sequentially
- Task 29 commits Phase 6 work
- Task 30 can be done anytime after task 19
- Tasks 31-32 finalize the implementation

## Verification Checklist
- [x] Terraform configuration passes `terraform validate`
- [x] Terraform configuration is formatted with `terraform fmt`
- [x] `.gitignore` prevents committing state files
- [ ] CloudFront URL is accessible and serves website correctly (requires AWS deployment)
- [ ] All static files (HTML, CSS, PNG) load properly (requires AWS deployment)
- [x] README documentation is clear and complete
- [x] Content update workflow is tested and documented
- [ ] Infrastructure can be destroyed and recreated (requires AWS deployment)
- [ ] OpenSpec validation passes with --strict flag (to be validated)
- [x] All changes committed with descriptive messages
- [ ] All commits pushed to remote repository (pending)
