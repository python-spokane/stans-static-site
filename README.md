# Stan's Static Site 
A static website about Stan the Snakes's adventures created using Python's MkDocs

## TODO
- [x] Add MkDocs site
- [ ] Write blog posts for
  - [ ] Adventure at Amazon
  - [ ] Adventure at Microsoft
- [ ] Add hosting info about 
  - [ ] AWS
  - [ ] Azure
  - [ ] GitHub Pages
- [ ] AWS diagram
- [ ] Add other ways to deploy to AWS links

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
- Grant the role or user created above permissions on the s3 bucket
