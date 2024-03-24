# S3

S3 is a solution based on object storage. We are having collections of data blobs.

flat system like no hierarchy and no schema.

A bucket can fit any size, type of data.

Requires a unique name.

## when to use this?

Needs to store big amount of data.

Needs to store unrelated data.

Needs to store big data that not suitable for DB records.

## Design?

Hash table in the cloud.

Objects bears with unique key.

No hierarchy but keys can use a logical path (like the idea of partition or bucket).

Objects data is automatically replicated across AWS data centres. Updates to a single key are atomic. They made the GET PUT LIST operation automatically see the last update. (MQ?)

## Public Buckets:

S3 is HTTP only, but you can use CloudFront CDN.

CloudFront caches contents of public bucket to reduce latency and use HTTPs.

It could be used to serve front end by using public bucket.(At coop, the POC was deployed by using before we made the final version)

## S3 CLI

```bash
# List objects in a bucket named myBucket

aws s3 ls s3://mybucket

# Copy a local file into a bucket

aws s3 cp file.txt s3://mybucket/file.txt

# sync everything locally in ./website with the bucket

aws s3 sync ./website s3://mybucket

```

## S3 node.js SDK

aws-sdk v3 is the most popular options so far.

For the code, looking into the fragment project of mine.

## A walk through of creating S3

## Minio and S3
