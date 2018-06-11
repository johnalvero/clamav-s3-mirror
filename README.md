# ClamAV S3 Mirror

## Diagram

![CodeBuild S3 Diagram](https://github.com/johnalvero/clamav-s3-mirror/blob/master/CodeBuild%20S3%20Mirror%20-%20Page%201.jpegg)

## Pre-requisites
- 1 S3 Bucket for hosting the AWS CodeBuild files (buildspec.yml). Let's call this source bucket
- 1 S3 Bucket where the ClamAV signature files will be uploaded. Let's call this target bucket
- Zip and upload this repo to the source bucket

## Create a CodeBuild Project
```
Project Name: Type any name
Source Provider: Amazon S3
Bucket: <The source bucket name>
S3 object key: <The object key to the zip file>
Environment image: Use an image managed by AWS CodeBuild
Build specification: Use the buildspec.yml in the source code root directory
Buildspec name: buildspec.yml
Certificate: Do not install any certificate
Artifacts Type: No Artifacts
Cache Type: No cache
Service Role: Create a service role in your account
VPC: No VPC
```

## Setup the CodeBuild Service Role
Make sure that the service role has permissions to upload to the target S3 bucket.

## Setup daily run
Once the project is successfully created, you may now setup Build Triggers to run it daily.
