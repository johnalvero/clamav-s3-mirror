# ClamAV S3 Mirror

## Diagram

![CodeBuild S3 Diagram](https://raw.githubusercontent.com/johnalvero/clamav-s3-mirror/master/diagram.jpeg)

## Create the buckets
```
# Source Bucket
aws s3 mb s3://<source-bucket>

# Target Bucket
aws s3 mb s3://<target-bucket>
```

## Create a CodeBuild Project
```

```

## Setup the CodeBuild Service Role
Make sure that the service role has permissions to upload to the target S3 bucket.

## Setup daily run
Once the project is successfully created, you may now setup Build Triggers to run it daily.

## S3 Website hosting
Enable the target bucket's website hosting and setup the correct public bucket policy.

## Agent / Clients Config
Tell freshclam to use the mirror. In /etc/clamav/freshclam.conf, make sure the following exists:
```
PrivateMirror <s3-url>
```
This overrides the DatabaseMirror configuration directive disabling DNS-based signature lookup.

## For improvement
If you want to go even further, you can setup AWS CloudFront in front of the S3 bucket serving the AV signatures.
