# Steps to create GCP Service Account and Key

## 1. Create Service Account

gcloud iam service-accounts create github-actions-sa \
 --display-name="GitHub Actions Service Account" \
 --description="Service account for GitHub Actions CI/CD"

## 2. Grant necessary permissions

# For Container Registry (GCR) push access

gcloud projects add-iam-policy-binding YOUR_PROJECT_ID \
 --member="serviceAccount:github-actions-sa@YOUR_PROJECT_ID.iam.gserviceaccount.com" \
 --role="roles/storage.admin"

# For Container Registry access

gcloud projects add-iam-policy-binding YOUR_PROJECT_ID \
 --member="serviceAccount:github-actions-sa@YOUR_PROJECT_ID.iam.gserviceaccount.com" \
 --role="roles/storage.objectAdmin"

# For creating repositories on push (if needed)

gcloud projects add-iam-policy-binding YOUR_PROJECT_ID \
 --member="serviceAccount:github-actions-sa@YOUR_PROJECT_ID.iam.gserviceaccount.com" \
 --role="roles/artifactregistry.admin"

## 2.1 Enable required APIs

gcloud services enable containerregistry.googleapis.com
gcloud services enable cloudbuild.googleapis.com

## 2.2 Create the repository manually (RECOMMENDED to avoid the error)

# Option A: Push a test image to create the repository

docker pull alpine:latest
docker tag alpine:latest gcr.io/YOUR_PROJECT_ID/ui-post-factory:init
docker push gcr.io/YOUR_PROJECT_ID/ui-post-factory:init

# Option B: Use gcloud to create the repository (if using Artifact Registry)

# gcloud artifacts repositories create ui-post-factory --repository-format=docker --location=us-central1

## 3. Create and download the key

gcloud iam service-accounts keys create ~/github-actions-key.json \
 --iam-account=github-actions-sa@YOUR_PROJECT_ID.iam.gserviceaccount.com

## 4. View the key content (copy this entire JSON)

cat ~/github-actions-key.json
