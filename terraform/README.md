# Terraform AWS Deployment for Simple Presence

This directory contains Terraform configuration for deploying the Simple Presence static website to AWS using S3 and CloudFront.

## Architecture

The deployment uses the following AWS services:
- **S3 Bucket**: Stores static website files (HTML, CSS, images)
- **CloudFront Distribution**: CDN for fast global delivery
- **Origin Access Identity (OAI)**: Secure access from CloudFront to S3

## Prerequisites

1. **AWS Account**: You need an AWS account with appropriate permissions
2. **Terraform CLI**: Install Terraform (>= 1.0)
   ```bash
   # macOS
   brew install terraform

   # Or download from https://www.terraform.io/downloads
   ```
3. **AWS Credentials**: Configure AWS credentials using one of these methods:
   - AWS CLI: `aws configure`
   - Environment variables: `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`
   - AWS credentials file: `~/.aws/credentials`

## Deployment Steps

### Initial Deployment

1. Navigate to the terraform directory:
   ```bash
   cd terraform
   ```

2. Initialize Terraform (downloads required providers):
   ```bash
   terraform init
   ```

3. Preview the changes that will be made:
   ```bash
   terraform plan
   ```

4. Apply the configuration to create AWS resources:
   ```bash
   terraform apply
   ```

   Type `yes` when prompted to confirm.

5. After deployment completes, Terraform will output the CloudFront URL:
   ```
   Outputs:
   cloudfront_url = "d1234567890abc.cloudfront.net"
   cloudfront_distribution_id = "E1234567890ABC"
   s3_bucket_name = "simple-presence-website-abcd1234"
   ```

6. Visit the CloudFront URL in your browser to view the deployed website.

### Updating Website Content

When you modify the HTML, CSS, or image files:

1. Run `terraform apply` to sync changes to S3:
   ```bash
   terraform apply
   ```

2. Invalidate the CloudFront cache to see changes immediately:
   ```bash
   aws cloudfront create-invalidation \
     --distribution-id <DISTRIBUTION_ID> \
     --paths "/*"
   ```

   Replace `<DISTRIBUTION_ID>` with the value from `terraform output cloudfront_distribution_id`.

### Infrastructure Teardown

To destroy all AWS resources and avoid ongoing costs:

```bash
terraform destroy
```

Type `yes` when prompted to confirm.

**Warning**: This will delete the S3 bucket, CloudFront distribution, and all associated resources. Make sure you have backups of any data you need.

## Configuration Variables

You can customize the deployment by creating a `terraform.tfvars` file (see `terraform.tfvars.example`):

- `aws_region`: AWS region for resources (default: `us-east-1`)
- `project_name`: Project name for resource naming (default: `simple-presence`)

## Outputs

After deployment, you can view outputs at any time:

```bash
terraform output
```

Available outputs:
- `cloudfront_url`: The CloudFront distribution URL for accessing your website
- `cloudfront_distribution_id`: Distribution ID (needed for cache invalidation)
- `s3_bucket_name`: Name of the S3 bucket

## Cost Estimate

Expected monthly costs for typical low-traffic usage:
- **S3 Storage**: ~$0.023 per GB/month (minimal for 3 files)
- **CloudFront**: Free tier includes 1TB data transfer out/month
- **Total**: Typically < $1/month for small static sites

CloudFront Free Tier (12 months for new AWS accounts):
- 1TB data transfer out
- 10,000,000 HTTP/HTTPS requests

## Security

- S3 bucket is **private** with all public access blocked
- CloudFront uses Origin Access Identity (OAI) for secure S3 access
- HTTPS is enforced (HTTP requests redirect to HTTPS)
- Server-side encryption enabled for S3 objects

## Troubleshooting

### Website not loading
- Wait 10-15 minutes after deployment for CloudFront to fully propagate
- Check CloudFront distribution status: `terraform show | grep status`

### Changes not appearing
- Invalidate CloudFront cache (see "Updating Website Content" above)
- CloudFront caches content for 1 hour by default

### Terraform state issues
- State is stored locally in `terraform.tfstate`
- **Do not** commit state files to Git (handled by `.gitignore`)
- For team environments, consider using remote state (S3 + DynamoDB)

## Future Enhancements

Potential improvements for this deployment:
- Custom domain with Route53
- SSL/TLS certificate via AWS Certificate Manager
- CI/CD pipeline for automated deployments
- Remote state backend for team collaboration
- Multi-environment support (dev/staging/prod)
- Automated CloudFront invalidation

## Files

- `main.tf`: Primary Terraform configuration
- `variables.tf`: Input variable definitions
- `outputs.tf`: Output value definitions
- `terraform.tfvars.example`: Example variable values
- `.gitignore`: Excludes state files and sensitive data from Git
