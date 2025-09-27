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

## 3. Create and download the key

gcloud iam service-accounts keys create ~/github-actions-key.json \
 --iam-account=github-actions-sa@YOUR_PROJECT_ID.iam.gserviceaccount.com

## 4. View the key content (copy this entire JSON)

cat ~/github-actions-key.json
