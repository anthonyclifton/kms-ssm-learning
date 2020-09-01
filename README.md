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

Terrform snippet to create a Customer Managed Key and then use it in a data block to encrypt an
arbitrary JSON object:

```
  resource "aws_kms_key" "oauth_config" {
    description = "oauth config"
    is_enabled  = true
    enable_key_rotation = true
  }

  data "aws_kms_ciphertext" "oauth" {
    key_id = "${aws_kms_key.oauth_config.key_id}"

    plaintext = <<EOF
    {
      "client_id": "e587dbae22222f55da22",
      "client_secret": "8289575d00000ace55e1815ec13673955721b8a5"
    }
    EOF
  }

  output "ciphertext" {
    value = "${data.aws_kms_ciphertext.oauth.ciphertext_blob}"
  }
```

Once you have the CMK set up, assuming you have privileges to use it, you can encrypt any file like so:

https://random.ac/cess/2017/02/04/simple-aws-cli-kms-encrypt-decrypt-example/

```
aws kms encrypt 
	--key-id [key-id] 
	--plaintext fileb://<(cat ~/plaintext.txt) 
	--query CiphertextBlob 
	--output text > ~/ciphertext.txt
```

Then you can use the same key-id to decrypt the file:

```
aws kms decrypt 
	--key-id [key-id]
	--ciphertext-blob fileb://<(cat ~/ciphertext.txt) 
	--output text 
	--query Plaintext > ~/decrypted.txt
```

Note: In the localstack implementation, anyone can encrypt using a key by default, but cannot decrypt
without an appropriate policy/role that allows them to decrypt.  This can be done with a aws_kms_grant
in Terraform.  However, localstack doesn't implement grant so I'm not able to try it.

