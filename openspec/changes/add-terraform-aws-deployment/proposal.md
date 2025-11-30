# Add Terraform AWS Deployment Infrastructure

## Summary
Add a Terraform configuration directory to enable deployment of the Simple Presence static website to AWS S3 with CloudFront as the front-end CDN.

## Why
The Simple Presence project currently has no deployment infrastructure. Static files can only be viewed locally, limiting the ability to share or host the website. Adding Terraform infrastructure-as-code enables:
- Reproducible AWS infrastructure provisioning
- Professional hosting via S3 and CloudFront CDN
- Global content delivery with low latency
- Secure, scalable static website hosting
- Infrastructure version control and documentation

## Context
The Simple Presence project currently consists of static files (`index.html`, `style.css`, `trashpanda.png`) that are only viewable locally. There is no deployment infrastructure configured. This change introduces Terraform infrastructure-as-code to provision AWS resources for hosting the static site.

## Scope
- Create `terraform/` directory with Terraform configuration files
- Configure AWS S3 bucket for static website hosting
- Configure CloudFront distribution for CDN delivery
- Set up proper S3 bucket policies and CloudFront origin access
- Include necessary Terraform variables and outputs
- Document deployment process

## Out of Scope
- CI/CD pipeline integration (future change)
- Custom domain configuration (future change)
- SSL certificate management beyond CloudFront default (future change)
- Multi-region deployment (future change)

## Questions & Clarifications
1. **AWS Region**: Should we default to a specific region (e.g., `us-east-1`) or make it configurable via variable?
   - **Decision**: Make region configurable via variable, default to `us-east-1`
2. **Bucket Naming**: Should bucket name be configurable or auto-generated?
   - **Decision**: Make bucket name configurable via variable with a sensible default pattern
3. **CloudFront Caching**: Should we configure specific cache behaviors or use defaults?
   - **Decision**: Use sensible defaults with cache headers for static assets
4. **State Management**: Should Terraform state be stored locally or in S3/remote backend?
   - **Decision**: Start with local state; remote backend can be added in future change

## Related Changes
None (first infrastructure change)

## Dependencies
- AWS account with appropriate permissions
- Terraform installed locally
- AWS CLI configured (for deployment)

