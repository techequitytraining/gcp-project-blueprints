# End User Documentation for Terraform Main.tf Module

## Overview

This Terraform module automates the setup of a Google Cloud Platform (GCP) project, including the creation of a billing budget, enabling necessary APIs, and configuring notification channels. It allows users to either create a new project or use an existing one, providing flexibility in managing resources on GCP.

## Features

- **Project Creation**: Optionally creates a new GCP project or uses an existing one based on user input.
- **Random Deployment ID**: Generates a unique deployment ID if not provided by the user.
- **API Management**: Enables a set of default APIs necessary for various GCP services.
- **Billing Budget Creation**: Allows the setup of a budget with alert thresholds to monitor spending.
- **Notification Channels**: Configures email notifications for budget alerts.
- **Resource Dependencies**: Ensures resources are created in the correct order to avoid conflicts.

## Module Structure

The module consists of several key components:

1. **Local Values**: Defines local variables for use throughout the configuration, including project selection, region, and APIs to enable.

2. **Data Sources**: 
   - Fetches available compute zones.
   - Retrieves information about existing GCP projects if not creating a new one.

3. **Resources**:
   - **Random ID Generation**: Generates a random ID if a deployment ID is not provided.
   - **Project Creation**: Creates a new GCP project if specified.
   - **Billing Budget**: Sets up a billing budget for the project.
   - **Pub/Sub Topic**: Creates a Pub/Sub topic for budget notifications.
   - **Notification Channels**: Configures email notification channels for budget alerts.
   - **API Services**: Enables necessary APIs for the project.

## Usage Instructions

### Prerequisites

- Ensure you have Terraform installed on your machine.
- Set up Google Cloud SDK and authenticate your account.
- Have the necessary IAM permissions to create projects and manage billing.

### Module Configuration

1. **Define Variables**: Create a `variables.tf` file to define the required input variables. Here are some key variables to include:

   ```hcl
   variable "create_project" {
     description = "Flag to create a new GCP project."
     type        = bool
   }

   variable "project_id_prefix" {
     description = "Prefix for the GCP project ID."
     type        = string
   }

   variable "billing_account_id" {
     description = "Billing account ID to link the project."
     type        = string
   }

   variable "create_budget" {
     description = "Flag to create a billing budget."
     type        = bool
   }

   variable "billing_budget_amount" {
     description = "Amount for the billing budget."
     type        = number
   }

   variable "billing_budget_notification_email_addresses" {
     description = "List of email addresses for budget notifications."
     type        = list(string)
   }
   ```

2. **Initialize Terraform**: Run the following command to initialize the Terraform workspace:

   ```bash
   terraform init
   ```

3. **Plan the Deployment**: Use the following command to see what resources will be created:

   ```bash
   terraform plan -var-file="your_variables_file.tfvars"
   ```

4. **Apply the Configuration**: Deploy the resources by running:

   ```bash
   terraform apply -var-file="your_variables_file.tfvars"
   ```

### Example Usage

Here’s an example of how to call the module in your Terraform configuration:

```hcl
module "gcp_setup" {
  source                = "./path_to_module"
  create_project        = true
  project_id_prefix     = "my-project"
  billing_account_id    = "your-billing-account-id"
  create_budget         = true
  billing_budget_amount  = 100
  billing_budget_notification_email_addresses = ["user@example.com"]
}
```

## Outputs

This module does not currently define any outputs. You may want to consider adding outputs for project IDs, budget details, or other relevant information.

## Conclusion

This Terraform module simplifies the process of setting up a GCP project with essential configurations, making it easier for users to manage their cloud resources effectively. By following the instructions above, users can quickly deploy a fully configured GCP environment tailored to their needs.

# End User Documentation for Terraform Filestore.tf Module

## Overview

This Terraform module automates the creation and management of Google Cloud Filestore instances and their backups. Filestore provides fully managed NFS file storage for applications that require a file system interface and a shared filesystem for data. This module allows users to define the configuration of Filestore instances, including capacity, network settings, and backup options.

## Features

- **Filestore Instance Creation**: Automatically creates a Filestore instance based on user-defined parameters.
- **File Share Configuration**: Configures file shares with specified capacity and access options.
- **Backup Management**: Creates backups for the Filestore instance to ensure data durability and recovery options.
- **Network Configuration**: Integrates with existing VPC networks to enable access to the Filestore instance.

## Module Structure

The module consists of the following key components:

1. **Local Values**: Defines local variables to format the name of the Filestore instance based on a random ID.

2. **Filestore Instance Resource**:
   - Creates a Filestore instance with specified properties such as name, location, tier, and file share configuration.
   - Configures NFS export options, including IP ranges and access modes.
   - Establishes network settings for the Filestore instance.

3. **Filestore Backup Resource**:
   - Creates a backup of the Filestore instance, including the source instance and file share name.

## Usage Instructions

### Prerequisites

- Ensure you have Terraform installed on your machine.
- Set up Google Cloud SDK and authenticate your account.
- Have the necessary IAM permissions to create Filestore instances and manage backups.

### Module Configuration

1. **Define Variables**: Create a `variables.tf` file to define the required input variables. Here are some key variables to include:

   ```hcl
   variable "create_filestore" {
     description = "Flag to create a Filestore instance."
     type        = bool
   }

   variable "filestore_tier" {
     description = "Tier for the Filestore instance (e.g., STANDARD, PREMIUM)."
     type        = string
   }

   variable "filestore_capacity" {
     description = "Capacity of the Filestore instance in GB."
     type        = number
   }

   variable "network_name" {
     description = "Name of the VPC network to connect the Filestore instance."
     type        = string
   }

   variable "subnet_cidrs" {
     description = "CIDR ranges for the subnets where the Filestore instance will be accessible."
     type        = map(object({
       gce_subnet_cidr_range = string
     }))
   }

   variable "regions" {
     description = "List of regions where the Filestore instance will be created."
     type        = list(string)
   }
   ```

2. **Initialize Terraform**: Run the following command to initialize the Terraform workspace:

   ```bash
   terraform init
   ```

3. **Plan the Deployment**: Use the following command to see what resources will be created:

   ```bash
   terraform plan -var-file="your_variables_file.tfvars"
   ```

4. **Apply the Configuration**: Deploy the resources by running:

   ```bash
   terraform apply -var-file="your_variables_file.tfvars"
   ```

### Example Usage

Here’s an example of how to call the module in your Terraform configuration:

```hcl
module "filestore_setup" {
  source                = "./path_to_module"
  create_filestore      = true
  filestore_tier        = "STANDARD"
  filestore_capacity     = 1024
  network_name          = "my-vpc-network"
  subnet_cidrs          = {
    "us-central1" = {
      gce_subnet_cidr_range = "10.0.0.0/24"
    },
    "us-west1" = {
      gce_subnet_cidr_range = "10.1.0.0/24"
    }
  }
  regions               = ["us-central1", "us-west1"]
}
```

## Outputs

This module does not currently define any outputs. You may want to consider adding outputs for the Filestore instance ID, backup ID, or other relevant information.

## Conclusion

This Terraform module simplifies the process of setting up and managing Google Cloud Filestore instances and their backups. By following the instructions above, users can quickly deploy a fully configured Filestore environment tailored to their needs, ensuring efficient data management and access in their applications.


