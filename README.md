# cloudrun-sample
cloudrun-sample

# Pre-requisites
Install Google Cloud command line SDK: https://cloud.google.com/sdk/docs/install and Github command line: https://github.com/cli/cli

# Authenticate gcloud
```shell
export PROJECT_ID=<your_project_id_here>
gcloud auth login
gcloud config set project $PROJECT_ID
```

# Setup project
*Using the instructions here:  (https://github.com/google-github-actions/setup-gcloud/blob/master/example-workflows/cloud-run/README.md)*

### Enable the Cloud Run API gcloud 
```shell
gcloud services enable run.googleapis.com
```

### Create a Google Cloud service account or select an existing one
```shell
gcloud iam service-accounts create github-actions
```

### Add the the following Cloud IAM roles to your service account
```shell
gcloud projects add-iam-policy-binding $PROJECT_ID --member serviceAccount:github-actions@${PROJECT_ID}.iam.gserviceaccount.com --role roles/run.admin
gcloud projects add-iam-policy-binding $PROJECT_ID --member serviceAccount:github-actions@${PROJECT_ID}.iam.gserviceaccount.com --role roles/iam.serviceAccountUser
gcloud projects add-iam-policy-binding $PROJECT_ID --member serviceAccount:github-actions@${PROJECT_ID}.iam.gserviceaccount.com --role roles/storage.admin
```

### Download a JSON service account key for the service account.
```shell
gcloud iam service-accounts keys create account.json --iam-account github-actions@${PROJECT_ID}.iam.gserviceaccount.com
```

### Add the following [secrets to your repository's secrets][gh-secret]:

```shell
# Login to Github
gh auth login

# List your Github Repos
gh repo list

# Setup an env var with your target repo
export GH_REPO=<your_github_repo_here>

# Then configure the secrets in the target repo
gh secret set GCP_PROJECT -r $GH_REPO
gh secret set GCP_SA_KEY -r $GH_REPO < account.json
```

# Run pipelines

List available workflow:
```shell
gh workflow list
```

To run a pipeline
```shell
gh workflow run <workflow_id>
```