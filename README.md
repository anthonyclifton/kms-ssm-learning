# AWS Secrets Manager/SSM with KMS 
## Notes and Examples

### KMS

KMS keys are region specific and can only be used in the region they're created in. AWS Managed Keys are free.
Customer generated keys are $1/month as of 8/31/2020.

* https://aws.amazon.com/kms/
* https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/kms_key
* https://medium.com/appsecengineer/a-guided-tour-of-the-aws-key-management-service-kms-as-code-4da580710e5b
