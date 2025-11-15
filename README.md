# SynologyCloudSyncS3Permissions
The package contains samples of how to configure Synology CloudSync for S3 with the minimum permissions required.

There's a lot of back advice out there on other guides (like granting `s3:*` or attaching any of the AWS Managed Policies - which will grant access to all buckets). 

## Important notes
- I've only considered the `Upload local changes only` setting
  - With `Don't remove files in the destination folder when they are removed in the source folder` selected
  - If you want to delete items, you will need to add on to the policy
- I do not specify any encryption settings in CloudSync (I want to rely on AWS to perform encryption)
- The policies have placeholders you must replace:
  - `<bucket_name>`: The bucket name
  - `<optional_path>`: Any optional prefix
  - `<key_name>`: The key alias (makes the policy more portable if using CDK)

## IAM/SynologyBackupMinimal.json
Contains a reasonable policy the grants access to the entire bucket
- Assumes you are using SSE-S3

## IAM/SynologyBackupRestrictive.json
Contains a policy statement that allows specifying a KMS key for using SSE-KMS
- Use `ForAllValues:StringLike` to use a wildcard alias (e.g. `alias/CMK-S3-*`)
- I attempted to restrict the `s3:ListBucket` with a conditional statement for `s3:prefix`. I got authorization failed errors.
