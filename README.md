# AWS Secrets Manager/SSM with KMS 
## Notes and Examples

### KMS

KMS keys are region specific and can only be used in the region they're created in. AWS Managed Keys are free.
Customer generated keys are $1/month as of 8/31/2020.

* https://aws.amazon.com/kms/
* https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/kms_key
* https://medium.com/appsecengineer/a-guided-tour-of-the-aws-key-management-service-kms-as-code-4da580710e5b

### Secrets Manager

You can use SSM Parameter Store which can store key-value pairs unencrypted or encrypted.  However, 
Secrets Manager is the preferred method now, as it stores all key-value pairs encrypted. Secrets Manager
also handles key rotation for you, has a low costs, and allows cross-account access to keys.

* https://www.1strategy.com/blog/2019/02/28/aws-parameter-store-vs-aws-secrets-manager/

* https://aws.amazon.com/secrets-manager/
* https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/secretsmanager_secret
* https://blog.gruntwork.io/a-comprehensive-guide-to-managing-secrets-in-your-terraform-code-1d586955ace1
