# Deploy AWS EC2 Instance, Key Pair, and Security Group with Terraform

### ARCHITECTURE DIAGRAM 🌟
<img width="1636" alt="Architecture" src="https://github.com/Sankalpjadhav/IaC-Project/assets/39724170/9880fa3e-509f-48b7-858c-5d40a001e809">

## AWS Account 🤖

If you don’t have an AWS account, you must create one. Go to the AWS website (https://aws.amazon.com/) and follow the instructions to create an account. Make sure to provide accurate billing information, even though we’re aiming for the Free Tier.

> _It’s crucial to verify the region where you’ll deploy resources. For instance, 🌎 I’ll be provisioning resources for this project in the us-east-1 region. If you create a resource in us-east-1 and forget to terminate it, it could turn into a disaster💰. Also, if you forget about resources in a region you’re not using, they’ll keep incurring 💰 charges. Double-checking the region can help you avoid unnecessary costs and ensure efficient resource management 🙌_

### 1. Install Terraform

If you haven’t already installed Terraform on your local machine. You can download it from the official website: https://developer.hashicorp.com/terraform/downloads

Make sure to set up environment variables so that you can use Terraform for all instances of the directory. After installing Terraform, you can confirm if it’s installed by opening a command prompt or terminal and typing: `terraform version`

![Terraform_Version](https://github.com/Sankalpjadhav/IaC-Project/assets/39724170/46924745-bb74-41f9-8cec-d3dc459d27e3)


### 2. Configure AWS Credentials

> _**Why do we need this step?**
> This step involves configuring AWS credentials, which is essential for Terraform to interact with our AWS account. Terraform uses these credentials to authenticate itself and make API calls to create and manage resources on our behalf._

I’ll be creating a new IAM user on AWS to enhance security and avoid using the admin account for regular tasks.

* **Create a New IAM User**
  - Log in to your AWS Management Console > Search “IAM”
  - In the IAM dashboard, click on “Users” in the sidebar
  - Click the “Create user” button > Choose a username for the new user
  - Create Group for “Admin Access” > Add user to the group
  - Click Next > Next > Click on the user that you’ve created just now
  - Click on “Create access key” > Select Use case as “Local code” > Next
  - A confirmation page will display the “Access key ID” and “Secret access key” for the user.
 
> _**Important:** Make sure to save the secret access key in a secure location. It’s only shown once. If you lose it, you’ll need to generate a new key pair._

* **Create a Directory for Your Project**

  This is where you’ll store your configuration files.

* **Use AWS credentials**

  You’re now ready to use these credentials to deploy an EC2 instance using Terraform. Terraform offers multiple ways to authenticate, but we’ll focus on three common methods: directly hardcoding credentials (not recommended), using the AWS CLI, and using environment variables.

  > _**Hardcoding credentials (not recommended):**_
  ```
  # Configure the AWS Provider
  provider "aws" {
    region     = "us-east-1"
    access_key = "AKIA----------"
    secret_key = "9dIj-------------------------"
  }
  ```
  > _**AWS CLI (recommended):**_

  By running the `aws configure` command, you can set your credentials for the current environment. Terraform will then use these credentials for authentication. Ensure you have the AWS CLI installed and configured.

  > _**Using Environment Variables:**_

  Environment variables are another secure way to provide credentials to Terraform. Set the following environment variables before running Terraform commands:

    - **Windows:**
      ```
      set AWS_ACCESS_KEY_ID=Your_Access_Key
      set AWS_SECRET_ACCESS_KEY=Your_Secred_Access_Key
      set AWS_DEFAULT_REGION=Your_AWS_Region
      ```
    - **Mac or Linux Flavours:**
      ```
      export AWS_ACCESS_KEY_ID=Your_Access_Key 
      export AWS_SECRET_ACCESS_KEY=Your_Secred_Access_Key 
      export AWS_DEFAULT_REGION=Your_AWS_Region
      ```
### 3. Write the Terraform Configuration to deploy an EC2 instance
Use a **provider.tf** file to define the configuration for a specific ☁️ or service provider. This file specifies the 🔑, endpoints, and other settings required for Terraform to interact with that provider’s API and manage resources.
> _Refer **provider.tf** file_

🔐 Creating an SSH key pair on your local system serves as a secure and convenient method for authenticating yourself when connecting to remote servers, such as instances on AWS 🔑

![Key_Pair](https://github.com/Sankalpjadhav/IaC-Project/assets/39724170/ec76d6b6-ad84-4fc1-bc0b-2bab06d8dd99)


> _This will generate **my-key** and **my-key.pub** files in our current directory. We will add the Public Key (**my-key.pub**) to the AWS instance and use the Private key (**my-key**) to authenticate ourselves when connecting to remote servers using SSH._

Now, we will be creating **variables.tf** and **terraform.tfvars** files which are used to manage and organize input variables for our infrastructure code. These files play distinct roles and help improve the maintainability and flexibility of our Terraform configurations.

> _The **variables.tf** file is used to define input variables that will be used throughout your Terraform configuration. Input variables allow you to parameterize your code and make it more dynamic._

> _Refer **variables.tf** file_

The **terraform.tfvars** file is used to assign specific values to the input variables defined in **variables.tf**. It provides a way to set values for these variables without modifying the main Terraform code. This is especially useful when working in a team or when deploying infrastructure for different environments.

> _Refer **terraform.tfvars** file_

The next step is to create an **instance.tf** file which automates the creation of an AWS key pair, a security group that allows SSH traffic, and an Ubuntu EC2 instance using the specified configurations. The resulting EC2 instance will have SSH access enabled through the associated key pair and security group rules.

> _Refer **instance.tf** file_

### 4. Ready to deploy resources
Now that our Terraform code is complete, it’s time to deploy the resources to AWS 😎 Here are the following commands to execute 💻:

_**terraform init**_

The `terraform init` command initializes a Terraform configuration in a working directory. 📥 It downloads necessary provider plugins, 📥 initializes backend configuration for remote state storage (if defined), 📥 and ensures required modules are installed and ready to use 🚀

_**terraform fmt**_

The `terraform fmt` command automatically formats Terraform configuration files to use standardized indentation, formatting, and line breaks 🔧

_**terraform validate**_

The `terraform validate` command checks Terraform configuration files for syntactical and structural errors 🔎 It verifies that all resource types, providers, variables, and other elements are correctly defined and referenced 👍

_**terraform plan**_

The `terraform plan` command creates an execution plan for our configuration, showing what changes will be made to our infrastructure when the configuration is applied 👀 Reviewing and confirming the expected changes is a crucial step before applying them 🔍

_**terraform apply**_

The `terraform apply` command uses our Terraform configuration to create, modify, or delete resources through our cloud provider’s API 🚀 It prompts us to review the changes planned by `terraform plan` and asks for confirmation before proceeding 💬 To avoid unexpected modifications, it’s important to carefully review the changes before confirming 👀


Boom! We have our instance up and running on AWS 🔥

![EC2_Instance](https://github.com/Sankalpjadhav/IaC-Project/assets/39724170/2b4f95ce-de8e-4071-a2dd-10d0ab076c64)


Go to instance > Record the 🌐 public IP address and utilize the given 🔑 command to access your instance through SSH:

```
ssh -i my-key ubuntu@PUBLIC-IP-OF-INSTANCE
```

Specify the path to our private key file (my-key) and replace PUBLIC-IP-OF-INSTANCE with our instance’s actual public IP.

Now, we are good to connect to our virtual machine 😎

> **IMPORTANT:**
When you’re done experimenting, remember to destroy the resources to avoid any potential costs.

_**terraform destroy**_

This will terminate the EC2 instance and the associated key-pair and security group. Remember to regularly check your AWS Free Tier usage dashboard to ensure that you’re not inadvertently incurring any charges. Also note that while I’ve provided a simplified example, there are more details and configurations you can include to enhance security, networking, and automation in your setup.

