Simple Bash scripts to automate the deployment of Pivotal on Google Cloud Platform. 

Step 1 Create Bastion:
git clone https://github.com/jasonbisson/terraform_pivotal_om.git

The create_bastion.sh script will deploy a bastion host and bucket used as a control point to deploy Pivotal. This can be kicked off from Cloud Shell, Laptop, or GCE instance with the required permissions to deploy GCE, Storage Bucket, and update project metadata.

Step 2 Validate Bastion host:
Validate the new environment with commands like terraform, om, and uaac. 

git clone https://github.com/jasonbisson/terraform_pivotal_om.git

Step 3 Generate Secrets for Install:
The generate_pivotal_secrets.sh will be run from the new bastion host to create a service account for Terraform, generate a self signed SSL cert, update the terraform.tfvars file, generate password for ops manager, and store the password in GCP KMS.

Step 4 Deploy GCP Infrastructure to support pivotal deployment:
The install_pivotal_infrastructure.sh scrikpt will download the terraform.tfvars file from the bastion bucket, deploy the gcp infrastructure using terraform, ops manager, and copy the tfstate file to the bastion bucket.

Step 5 Deploy Bosh Director via Ops Manager:
The install_pivotal_director.sh script will initialize authetication for Ops manager with the password via KMS and deploy the bosh director using OM CLI commands.

Step 6 Deploy Elastic Runtime

install_pivotal_elastic_runtime.sh will download the ERT tile/stemcell, upload the ERT tile/stemcell, configure ERT, and deploy using OM tool.

Requirements:
GCP project with adequate quotas to deploy Pivotal Small or Large. 

Permission to create a bastion host, bucket, and service account.

Update required variables in project metadata. There are a lot of variables, but it's intentional to provide flexibilty. 

TO DO's:
Use a CI/CD pipeline to deploy (Concourse, Jenkins, Travis??) or build a simple wrapper.

Provide option to not generate a self signed cert

Move pivnet token to KMS

Reduce permission from owner to least privilege

Create a script to add required metadata
