---
title: 'Setting a MinIO bucket for anonymous download'
date: '2020-12-24'
draft: false
slug: 'minio-bucket-anonymous-download'
tags: ["MinIO", "S3"]
categories: ["DevOps"]
excerpt: MinIO is a high-performance object storage compatible with Amazon S3 API.
image: '/images/minio.png'
---

MinIO is a high-performance object storage that can be used for serving static assets for your web application or any other kind of media assets.

It's very simple to install MinIO in a Caprover server, because it's available as a "One Click App".

Once installed, you can use the web interface (MinIO Browser) to simply create a new bucket.

Imagine that your web app needs to store some documents (for example a job offer) in the bucket, allowing people to download the document.

As explained in the <a href="https://docs.min.io/docs/minio-client-complete-guide.html" target="_blank">MinIO Client Complete Guide</a>, you can use the 'download' policy:

    mc policy set download myminioserver/job-offers/
    Access permission for ‘myminioserver/job-offers/’ is set to 'download'

Imagine that you upload a file called 'job-100-offer.pdf'. With the complete file path, you can download the document:

https://myminioserver/job-offers/job-100-offer.pdf

The problem is that also "directory listing" is enabled, which means that if you reach the root directory:

https://myminioserver/job-offers/

you'll get all the files availables in directory.
If the requirement is that directory listing is denied, then you can use a custom policy.
It seems difficult but it's actually very simple.

First, get the actual policy you set with the command above:

    mc policy get-json myminioserver/job-offers > ~/Desktop/policy.json

This is the content of the policy.json file:

<script src="https://gist.github.com/thomascenni/82d5d34839892c1ff7e08c0a6d8ed19b.js"></script>

Then, looking at the first Action

"Action": ["s3:GetBucketLocation", "s3:ListBucket"],

simply remove the content "s3:ListBucket", save the file and update the policy with the command:

    mc policy set-json ~/Desktop/policy.json myminioserver/job-offers

You're done. Only if you know the full path of the file, you'll be able to download it.
If you try a directory listing, you'll get a AccessDenied error.

