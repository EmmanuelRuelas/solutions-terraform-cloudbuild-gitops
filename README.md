# Managing infrastructure as code with Terraform, Cloud Build, and GitOps

This is the repo for the [Managing infrastructure as code with Terraform, Cloud Build, and GitOps](https://cloud.google.com/solutions/managing-infrastructure-as-code) tutorial. This tutorial explains how to manage infrastructure as code with Terraform and Cloud Build using the popular GitOps methodology. 

## Configuring your **dev** environment

Just for demostration, this step will:
 1. Configure an apache2 http server on network '**dev**' and subnet '**dev**-subnet-01'
 2. Open port 80 on firewall for this http server 

```bash
cd ../environments/dev
terraform init
terraform plan
terraform apply
terraform destroy
```

## Promoting your environment to **production**

Once you have tested your app (in this example an apache2 http server), you can promote your configuration to prodution. This step will:
 1. Configure an apache2 http server on network '**prod**' and subnet '**prod**-subnet-01'
 2. Open port 80 on firewall for this http server 

```bash
cd ../prod
terraform init
terraform plan
terraform apply
terraform destroy
```

///////////////////////////////////////////////////////////////////////

Compute Engine API

Comandos activar las Apis de GCP

    gcloud services enable cloudbuild.googleapis.com compute.googleapis.com

Comandos para crear un Cloud Storage con nombre del proyecto-tfstate

    PROJECT_ID=$(gcloud config get-value project)
    gsutil mb gs://${PROJECT_ID}-tfstate

Al crear este storage es necesario configurar los siguientes archivos:

    terraform.tfvars y backend.tf

En Cloud Shell, recupera el correo electrónico de la cuenta de servicio de Cloud Build de tu proyecto:

    CLOUDBUILD_SA="$(gcloud projects describe $PROJECT_ID \
        --format 'value(projectNumber)')@cloudbuild.gserviceaccount.com"

Otorga el acceso requerido a tu cuenta de servicio de Cloud Build:

    gcloud projects add-iam-policy-binding $PROJECT_ID \
        --member serviceAccount:$CLOUDBUILD_SA --role roles/editor
