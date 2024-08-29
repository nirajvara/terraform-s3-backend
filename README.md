# terraform-s3-backend
terraform-s3-backend
## Create bucket
aws s3api create-bucket --bucket terraform-state-bucket-new1 --region ap-south-1
aws s3api put-bucket-versioning --bucket my1-terraform-state-bucket --versioning-configuration Status=Enabled
## create Dynamo table
aws dynamodb create-table --table-name my-dynamodb-table --attribute-definitions AttributeName=LockID,AttributeType=S --key-schema AttributeName=LockID,KeyType=HASH --provisioned-throughput ReadCapacityUnits=5,WriteCapacityUnits=5

### provider.tf 
terraform {
  backend "s3" {
    bucket         = "terraform-state-bucket-new1"  # Replace with your bucket name
    key            = "terraform.tfstate"  # This is the path to the state file in the bucket
    region         = "ap-south-1"  # Replace with your region
    encrypt        = true  # Encrypt the state file at rest
    dynamodb_table = "my-dynamodb-table"  # Optional: For state locking and consistency checking
  }
}

provider "aws" {
  region = "ap-south-1"  # Replace with your preferred region


