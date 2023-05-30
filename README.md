# COSC2759 Assignment 2

## Student details

- Full Name: **Selamawit Tewelde Birhane**
- Student ID: **s3894053**

--- **Diagrams link**

<img src="img/1infra-diagram.drawio.png" style="width: 400px;" />
<img src="img/1loadBalancer.drawio.png" style="width: 400px;" />
<img src="img/1process-diagram.drawio.png" style="width: 400px;" />


**described and justified solutions**

--To achieve an automated deployment process for the Foo app, including infrastructure, i will use Terraform, Ansible, and Docker with AWS services to create and configure the necessary infrastructure. will also use GitHub and GitHub Actions to automate the deployment process.

--**Terraform**
-The Terraform tool will be used to define and deploy the infrastructure components required to run the application.
Use Terraform to deploy the required infrastructure, including the EC2 instances and database instance. will define the infrastructure components using Terraform code and deploy them using the AWS provider.
Create a new folder named infra.
Within the terraform, create a file named main.tf. This file will contain the main Terraform code to deploy the infrastructure. defining the provider block for AWS, create a new EC2 instance by adding codes, and initialize the Terraform workspace and deploy the infrastructure.

-- **Ansible**
-The Ansible, After the infrastructure components have been deployed, i will use Ansible to configure the EC2 instances with the necessary components and settings. will use Ansible playbooks to install Docker and other required software components, configure the security groups, and deploy and configure the application and database containers.
 However, if we put the database container on a separate machine then obviously, we can't use docker compose. Something to think about.
 will use to build process using the Docker Buildx plugin.

create ansible folder, create inventory.yml file, this file will contain the inventory of EC2 instances. also create playbook.yml in the ansible folder This file will contain the Ansible playbook that will use to configure the EC2 instances to run the app and database containers. 
-
-will deploy the app container on two identical EC2 instances behind a load balancer, with the database running on a separate EC2 instance. build a Docker image using the docker buildx command.

--**load balancer**
-to get load balancer with two instances behind it:
traffic will go through load balancer using DNS name / IP address.
loadbalancer resources, like subnets and VPC
listener group target group
target group attachment for each instance
subnet id, to deploy load balancer in to 
VPC id, to attach target group to

--**S3 bucket**
TO ensure that the application remains available even if one of the instances fails, will store the Terraform state, i will use an S3 bucket remote backend. 
This will enable us to manage the state of the infrastructure deployment and share it.
-create a separate Terraform configuration file, call it state-bucket-infra.tf with the given code, this will ensure that terraform uses the S3 bucket as the remote backend for state storage.
Run terraform init in the directory where terraform configurations are located. This will initialize the backend configuration and create the necessary infrastructure resources (S3 bucket and DynamoDB table) for state storage.
By following the steps above, will configure terraform to use the S3 bucket as the remote backend for storing the state file (terraform.tfstate). The state file will be automatically uploaded to the S3 bucket during terraform apply and retrieved during subsequent Terraform 

--
in the Amazon S3 service.
Create bucket
Provide a unique bucket name .
Select the region where want to create the bucket.
Choose the desired settings for bucket properties, such as versioning, logging, and tags.
Configure the bucket permissions

Once the bucket is created, can configure Terraform backend to use it. 
In main Terraform configuration file ( main.tf), add code block
and will save the updated main.tf file and run terraform init to initialize the backend with the ClickOps S3 bucket. 
Terraform will prompt to confirm the backend initialization. Confirm the initialization to proceed.

With the S3 bucket configured as the remote backend for Terraform state, Terraform will store the state file in the S3 bucket, enabling collaboration and state management for your infrastructure deployments.


--**Script**
-Set up environment variables, will need to set up environment variables for AWS credentials and any other parameters need for deployment.
 use file script to choose AMI, create the key pair to use for SSH access, an EC2 instances, specify the instance type, and security groups, the subnet to launch the instance in, and user data script that will be executed when the instance starts up, it is responsible for installing and configuring the necessary software, including Docker, and for starting the application container.
 operating system for the EC2 instance, update the package cache, install Docker using and start the Docker service. then download and run the app container, passing in the DB_PASSWORD environment variable and mapping port 80 in the container to port 80 on the host.
