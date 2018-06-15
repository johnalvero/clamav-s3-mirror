# ClamAV S3 Mirror

## Diagram

![CodeBuild S3 Diagram](https://raw.githubusercontent.com/johnalvero/clamav-s3-mirror/master/diagram.jpeg)

## Requirements
  - AWS cli setup properly
  - Clone this repo `git clone https://github.com/johnalvero/clamav-s3-mirror.git`

## Create the buckets
```
# Source Bucket
aws s3 mb s3://<source-bucket>

# Target Bucket
aws s3 mb s3://<target-bucket>
```
## Pack and send the source files to the source bucker
Before doing this step, make sure to update the target bucket location in buildspec.yml
```
zip clamav-mirror.zip buildspec.yml clamdownloader.pl
aws s3 cp clamav-mirror.zip s3://<source-bucket>
```
## Create the Service Role
Open put-role-policy.json file and modify the `<source-bucket> and <target-bucket>` parameters.
```
# Create service role. Take note of the role ARN as this will be needed in create-project.json
aws iam create-role --role-name <codebuild-clamav-mirror-role> --assume-role-policy-document file://create-role.json

aws iam put-role-policy --role-name <codebuild-clamav-mirror-role> --policy-name CodeBuildServiceRolePolicy --policy-document file://put-role-policy.json
```

## Create a CodeBuild Project
Edit the file create-project.json, update the file with the necessary parameters. The serviceRole parameter is the ARN from the create-role command.
```
aws codebuild create-project --cli-input-json file://create-project.json
```
At this point, you may already test the build process through CodeBuild service page in the AWS Management Console. If the the previous steps are done successfully, you should see files in the target bucket already after the build is finished.

## Setup website hosting on the target bucket
```
aws s3 website s3://<target-bucket>/ --index-document index.html

# Modify target-bucket-policy.json to suit your configuration
aws s3api put-bucket-policy --bucket <target-bucket> --policy file://target-bucket-policy.json
```



## Agent / Clients Config
Tell freshclam to use the mirror. In /etc/clamav/freshclam.conf, make sure the following exists:
```
PrivateMirror <s3-url>
```
This overrides the DatabaseMirror configuration directive disabling DNS-based signature lookup.

## For improvement
If you want to go even further, you can setup AWS CloudFront in front of the S3 bucket serving the AV signatures.
