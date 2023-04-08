# What is Google Cloud Storage? Explain how GCP Storage buckets work and how they can be used to store and manage data. How do you create and manage GCP Storage buckets using the GCP Console or gcloud CLI? Provide an example of a script to upload files to a GCP Storage bucket.

Google Cloud Storage is a cloud-based object storage service that allows users to store, access and manage data in the cloud. It offers a highly durable and available storage infrastructure that scales to meet the needs of applications.

GCP Storage buckets are containers that hold your data. They are a central concept in GCP Storage and can be used to organize, manage and control access to your data. Buckets can be accessed from anywhere in the world via an HTTP/HTTPS interface or a client library.

To create a GCP Storage bucket using the GCP Console:

1. Open the GCP Console and go to the Storage browser.
2. Click the "Create bucket" button.
3. Enter a name for your bucket and choose the storage class.
4. Choose the location where you want your bucket to be stored.
5. Choose the default access control for your bucket.
6. Click the "Create" button to create your bucket.

To create a GCP Storage bucket using the gcloud CLI:

```bash
gcloud storage buckets create [BUCKET_NAME]
```

To upload files to a GCP Storage bucket using the gcloud CLI:

```bash
gsutil cp [SOURCE_FILE] gs://[BUCKET_NAME]/
```

Here's an example script that uploads a file to a GCP Storage bucket using the gcloud CLI:

```bash
#!/bin/bash

# Set variables
BUCKET_NAME=my-bucket
SOURCE_FILE=/path/to/file.txt

# Create bucket
gsutil mb gs://$BUCKET_NAME

# Upload file to bucket
gsutil cp $SOURCE_FILE gs://$BUCKET_NAME/
```

This script creates a new GCP Storage bucket called "my-bucket" and uploads a file called "file.txt" to the bucket.