# Design: Terraform AWS Deployment Infrastructure

## Architecture Overview

The deployment infrastructure consists of:

1. **S3 Bucket**: Stores static website files (HTML, CSS, images)
2. **CloudFront Distribution**: CDN that serves content from S3 with global edge locations
3. **Origin Access Identity (OAI)**: Restricts S3 bucket access to CloudFront only

## Component Design

### S3 Bucket

- **Purpose**: Host static website files
- **Configuration**:
  - Private bucket (not public website hosting)
  - Website configuration with `index.html` as index document
  - Bucket versioning disabled (for simplicity)
  - Server-side encryption enabled
- **Access**: Restricted to CloudFront via OAI

### CloudFront Distribution

- **Purpose**: Global CDN for low-latency content delivery
- **Configuration**:
  - Origin: S3 bucket via OAI
  - Default root object: `index.html`
  - IPv6 enabled
  - HTTPS only (redirect HTTP to HTTPS)
  - Default cache behavior for static assets
  - Price class: Use default (all edge locations)

### Origin Access Identity

- **Purpose**: Secure S3 bucket access
- **Configuration**: Standard OAI with descriptive comment

## File Structure

```
terraform/
├── main.tf              # Main configuration, provider setup
├── variables.tf          # Input variables
├── outputs.tf           # Output values (bucket name, CloudFront URL)
├── s3.tf                # S3 bucket resource
├── cloudfront.tf        # CloudFront distribution and OAI
└── README.md            # Deployment instructions
```

## Security Considerations

1. **S3 Bucket Privacy**: Bucket is private, accessible only via CloudFront OAI
2. **HTTPS Enforcement**: CloudFront redirects all HTTP to HTTPS
3. **No Public Access**: Direct S3 access is blocked

## Cost Considerations

- S3 storage: Minimal cost for small static site
- CloudFront: Pay-per-use data transfer (first 1TB free tier)
- No EC2 or other compute resources required

## Trade-offs

1. **Local State vs Remote Backend**

   - **Chosen**: Local state (simpler initial setup)
   - **Trade-off**: Team collaboration requires remote backend (future change)

2. **S3 Website Hosting vs CloudFront**

   - **Chosen**: CloudFront with private S3
   - **Trade-off**: More complex but better performance and security

3. **Manual Deployment vs Automation**
   - **Chosen**: Manual Terraform apply
   - **Trade-off**: CI/CD integration deferred to future change

## Future Enhancements

- Remote state backend (S3 + DynamoDB)
- Custom domain with Route 53
- SSL certificate via ACM
- CI/CD pipeline for automated deployments
- CloudFront invalidation automation
