# Proposal: Add AWS Terraform Deployment

## Change ID
`add-aws-terraform-deployment`

## Summary
Add Terraform infrastructure-as-code configuration to deploy the Simple Presence static website to AWS using S3 for storage and CloudFront for content delivery. The Terraform configuration will be located in a `/terraform` directory at the project root.

## Why
The Simple Presence website currently lacks a production deployment mechanism. To make the site publicly accessible and professionally hosted, we need cloud infrastructure. Terraform provides a declarative, version-controlled approach to managing AWS resources, making the deployment reproducible and maintainable. The S3 + CloudFront architecture is industry-standard for static sites, offering high availability, global CDN distribution, and cost-effectiveness.

## Motivation
Currently, the Simple Presence website can only be viewed by opening the `index.html` file locally in a browser. This change enables:
- **Production hosting**: Deploy the site to a public URL accessible from anywhere
- **Scalability**: CloudFront CDN ensures fast load times globally
- **Infrastructure as Code**: Terraform provides reproducible, version-controlled infrastructure
- **Professional deployment**: S3 + CloudFront is a standard pattern for static site hosting

## Scope
This change introduces infrastructure configuration without modifying the existing website code. The deployment will:
- Create an S3 bucket for hosting static files
- Configure CloudFront distribution for CDN delivery
- Set up proper security and access controls
- Provide outputs for accessing the deployed site

## Out of Scope
- CI/CD automation (future enhancement)
- Custom domain configuration (future enhancement)
- SSL/TLS certificates via ACM (future enhancement)
- Multi-environment support (dev/staging/prod) (future enhancement)

## Capabilities Introduced
1. **terraform-infrastructure**: Core Terraform configuration for S3 and CloudFront
2. **deployment-workflow**: Documentation and processes for deploying the site

## Dependencies
- AWS account with appropriate credentials
- Terraform CLI installed locally
- No changes to existing HTML/CSS/image files

## Risks and Mitigations
- **Risk**: AWS costs for S3 storage and CloudFront traffic
  - **Mitigation**: CloudFront Free Tier includes 1TB data transfer out per month; S3 costs are minimal for static sites
- **Risk**: Terraform state management
  - **Mitigation**: Document local state file location; recommend remote state backend as future enhancement

## Success Criteria
- Terraform configuration successfully creates S3 bucket and CloudFront distribution
- Static site files can be uploaded to S3 via Terraform
- CloudFront URL serves the website correctly with all assets loading
- Infrastructure can be destroyed and recreated via Terraform commands
