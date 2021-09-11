# Scripts to Deploy MongoDB with Terraform
Deploying a MongoDB cluster on Atlas using Terraform


Installing Terraform

```
brew install terraform
```

Inside Atlas create your public and private API keys 

Create a the terraform.tfvars file and copy the following 

```
public_key = “############”
private_key  = “#############”
atlasprojectid = “ATLAS_PROJECT_ID”
cluster_region = “EU-WEST-3"
atlas_provider_name = “AWS”
atlas_provider_instance_size_name = “M10"
auto_scaling_disk_gb_enabled = true
mongo_db_major_version   	= “4.4”
mongodb_atlas_database_username = “****”
mongodb_atlas_database_user_password = “*****”
```

Get a api key and then fill in the param file (previously created)


Replace the public and private key and the atlasprojectId:

```
public_key = "ATLAS_PRIVATE_KEY"
private_key  = "ATLAS_PRIVATE_KEY"
atlasprojectid = "ATLAS_PROJECT_ID"
cluster_region = "EU-WEST-3"
atlas_provider_name = "AWS"
atlas_provider_instance_size_name = "M10"
auto_scaling_disk_gb_enabled = true
mongo_db_major_version       = "4.4"
mongodb_atlas_database_username = "DEFINE_MY_USERNAME"
mongodb_atlas_database_user_password = "DEFINE_MY_PASSWORD"
```

Then create “main.tf” file and change all “_NAME_” to your own

```
terraform {
 required_providers {
   mongodbatlas = {
     source = "mongodb/mongodbatlas"
     version = "0.7.0"
   }
 }
}

provider "mongodbatlas" {
 public_key  = var.public_key
 private_key = var.private_key
}

resource "mongodbatlas_cluster" "_NAME_" {
 project_id              = var.atlasprojectid
 name                    = "_NAME_"
 num_shards                   = 1
 replication_factor           = 3
 provider_backup_enabled      = true
 auto_scaling_disk_gb_enabled = var.auto_scaling_disk_gb_enabled
 mongo_db_major_version       = var.mongo_db_major_version
 //Provider settings
 provider_name               = var.atlas_provider_name
 provider_instance_size_name = var.atlas_provider_instance_size_name
 provider_region_name        = var.cluster_region
 }

output "connection_strings" {
value = mongodbatlas_cluster.eugene-terraform.connection_strings
}

resource "mongodbatlas_database_user" "_NAME_" {
 username           = var.mongodb_atlas_database_username
 password           = var.mongodb_atlas_database_user_password
 project_id              = var.atlasprojectid
 auth_database_name = "admin"
 roles {
   role_name     = "read"
   database_name = "admin"
 }
 roles {
   role_name     = "readWrite"
   database_name = "_NAME_"
 }
}
```


Create the “variables.tf” file:

```
variable "public_key" {
  description = "The public API key for MongoDB Atlas"
}
variable "private_key" {
  description = "The private API key for MongoDB Atlas"
}
variable "atlasprojectid" {
    description = "The Atlas Project ID used to create the cluster "
}
variable "cluster_region" {
    description = "The Atlas Project cluster region"
}
variable "atlas_provider_name" {
    description = "The Atlas cloud provider name"
}
variable "atlas_provider_instance_size_name"{
    description = "The Atlas provider instance size name"
}
variable "auto_scaling_disk_gb_enabled"{
    description = "auto scaling option"
}
variable "mongo_db_major_version"{
    description = "the MongoDB Version"
}
variable "mongodb_atlas_database_username"{
    description = "MongoDB Atlas DB username" 
}
variable "mongodb_atlas_database_user_password"{
    description = "MongoDB Atlas DB password" 
}
```

#Then once the files have been created, navigate to the folder and run these 3 commands

```
Run the command “terraform init”
Run the command “terraform plan”
Run the commande “terraform apply”
```


