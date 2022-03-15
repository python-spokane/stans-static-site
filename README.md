# Stan's Static Site <!-- omit in toc -->
A static website about Stan the Snakes's adventures created using Python's MkDocs

- [TODO](#todo)
- [Getting Started](#getting-started)
  - [Create virtual environment](#create-virtual-environment)
  - [Activate virtual environment](#activate-virtual-environment)
  - [Install dependencies](#install-dependencies)
  - [Debug site](#debug-site)
  - [Build site](#build-site)
- [Azure](#azure)
  - [Azure blob storage static website](#azure-blob-storage-static-website)
    - [Resources](#resources)
  - [Azure Static Web App](#azure-static-web-app)
    - [Resources](#resources-1)
- [AWS S3 Hosting](#aws-s3-hosting)
  - [Troubleshooting](#troubleshooting)
    - [For more indepth Python + AWS see Python on AWS - *Stan approved*](#for-more-indepth-python--aws-see-python-on-aws---stan-approved)

## TODO
- [x] Add MkDocs site
- [ ] Write blog posts for
  - [ ] Adventure at Amazon
  - [ ] Adventure at Microsoft
- [ ] Add hosting info about 
  - [x] AWS
  - [x] Azure
  - [ ] GitHub Pages
- [x] AWS diagram
- [x] Add other ways to deploy to AWS links

## Getting Started

### Create virtual environment
```python
python -m virtualenv .venv
```

### Activate virtual environment  
Bash
```
source .venv/bin/activate
```

PowerShell
```
.\.venv\Scripts\activate.ps1
```

### Install dependencies
```
pip install -r requirements.txt
```

### Debug site
```
cd website
mkdocs serve
```

### Build site
```
cd website
mkdocs build
```

## Azure

### Azure blob storage static website
```powershell
# create resource group
az group create --name spug --location westus2

# create storage account
az storage account create --name stansstaticstorage --resource-group spug --location westus2 --sku Standard_RAGRS --kind StorageV2

# list storage account keys
az storage account keys list --resource-group spug --account-name stansstaticstorage

# set "--auth-mode key" key
$Env:AZURE_STORAGE_KEY = "<account_storage_key>"

# configure storage account to use a static website
az storage blob service-properties update --account-name stansstaticstorage --static-website --404-document 404.html --index-document index.html

# upload static site files
az storage blob upload-batch -s .\website\site\ -d '$web' --account-name stansstaticstorage

# query website URL
az storage account show -n stansstaticstorage -g spug --query "primaryEndpoints.web" --output tsv

# add CNAME to domain provider -- https://domains.google.com/
# configure custom domain name
az storage account update -g spug --name stansstaticstorage --custom-domain "blob.stansadventures.com" --use-subdomain false

## other custom domain options include using Azure CDN
```

[https://blob.stansadventures.com/](https://blob.stansadventures.com/)

#### Resources
- [Host a static website on Blob Storage](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blob-static-website-host)
- [Map a custom domain to an Azure Blob Storage endpoint](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-custom-domain-name?tabs=azure-cli#map-a-custom-domain-with-https-enabled)
- [Integrate a static website with Azure CDN](https://docs.microsoft.com/en-us/azure/storage/blobs/static-website-content-delivery-network)

### Azure Static Web App

```powershell
# create static web app from git repo
az staticwebapp create --resource-group spug --source https://github.com/python-spokane/stans-static-site --branch main --location westus2 --name stans-static-site

# query hostname
az staticwebapp show --name stansstaticsite --query "@.defaultHostname"

# set custom domain
az staticwebapp hostname set --name stansstaticsite --hostname "hello.stansadventures.com" --no-wait
```

#### Resources
- [VS Code extension](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurestaticwebapps)
- [Quickstart: Building your first static site with Azure Static Web Apps](https://docs.microsoft.com/en-us/azure/static-web-apps/getting-started?tabs=vanilla-javascript)

## AWS S3 Hosting
- Use the following user guide to setup a simple s3 bucket to host a static website
  - [Hosting a static website using Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/WebsiteHosting.html)
- Setup AWS IAM User
  - As a best practice create an AWS IAM User and allow only it to write to the s3 bucket for deploying the site
  - Create a secret to store the AWS access keys for use in deploying the site via [Actions](https://github.com/python-spokane/stans-static-site/actions)
- Or *even better* [configure OIDC in AWS](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_create_oidc.html) and use [GitHub's OIDC provider to assume a role](https://github.com/aws-actions/configure-aws-credentials#assuming-a-role) for deploying the site
  - [Create a role for federation](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-idp_oidc.html)
  - [Create a trust policy](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create-console.html)
  - Create a Web Identity IAM role and associate the trust policy created in the previous step
- Grant the role or user created above permissions on the s3 bucket via a bucket policy (you really only want public *read* access)
  - Use the [AWS Policy Generator](https://awspolicygen.s3.amazonaws.com/policygen.html) to quickly create a secure policy
### Troubleshooting
- [Access denied error when using s3 static website](https://aws.amazon.com/premiumsupport/knowledge-center/s3-static-website-endpoint-error/)
#### For more indepth Python + AWS see [Python on AWS](https://aws.amazon.com/developer/language/python/) - *Stan approved*
