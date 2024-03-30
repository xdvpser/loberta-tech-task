# Loberta technical task

### Description
```
Here is the *TEST TASK*:

1. Using https://github.com/getsops/sops encrypt and decrypt example file.

test.enc.yaml
database:
    login: test
    password: 123
 cache:
    conn_str: 123@example.com:123/qwerty

2. Use encryption with GCP KMS and Application Default Credentials.
3. Setup a gcloud account with trial mode or use an existing one. 
4. Write a short documentation file .md with instructions on how to set up a gcloud account to use it with sops tool.

If you have any questions, please let me know.
```
### Result
**Prerequisites**
- Google account (and also credit or debit card)
- sops CLI, follow [installation instructions](https://github.com/getsops/sops/releases)
- gcloud CLI, follow [installation instructions](https://cloud.google.com/sdk/docs/install)

**Steps**
1. Setup Google Cloud account:
    1. Go to https://cloud.google.com/?hl=en and click on Start Free button
    2. Fill out all the required forms
    3. After confirming payment method you are ready to go
2. Configure **gcloud** CLI:
    1. Run in terminal: `gcloud auth login`. Browser will open login page from
        where you have to choose Google account.
    2. Get project ID: `gcloud projects list --format "value(projectId)"`
    3. Set project ID for CLI: `gcloud config set project <PROJECT_ID>`
    4. Configure *Application Default Credentials*: `gcloud auth application-default login`.
        The process is the same as with default gcloud login.
    5. Enable Cloud KMS service: `gcloud services enable cloudkms.googleapis.com`
    6. Wait a few minutes for actions to propagate.
3. Configure **sops** CLI:
    1. Create keyring: `gcloud kms keyrings create sops --location global`
    2. Create a key: `gcloud kms keys create sops-key --location global
        --keyring sops --purpose encryption`
    3. Get KMS ResourceID: `gcloud kms keys list --location global --keyring sops
        --format "value(name)"`
4. Work with files:
    1. Encrypt: `sops -e --gcp-kms <KMS ResourceID> test.yaml > test.enc.yaml`
    2. Edit encrypted file: `sops test.enc.yaml`
    3. Decrypt file: `sops -d test.enc.yaml`
