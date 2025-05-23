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


# End User Documentation for Terraform Firestore.tf Module

## Overview

This Terraform module automates the creation and management of a Google Cloud Firestore database along with a daily backup schedule. Firestore is a fully managed, scalable NoSQL document database designed for rapid development and real-time synchronization. This module enables users to configure Firestore instances with specific settings and automatically set up daily backups for data protection.

## Features

- **Firestore Database Creation**: Automatically provisions a Firestore Native database with customizable project and regional settings.
- **Optimistic Concurrency Mode**: Configures Firestore to use optimistic concurrency control for conflict management.
- **Point-in-Time Recovery**: Enables point-in-time recovery to restore data to any point within the retention period.
- **Backup Scheduling**: Sets up a daily backup schedule with configurable retention time to ensure data durability.
- **Deletion Policies**: Supports deletion protection settings and policies for safe resource management.

## Module Structure

The module consists of the following key components:

1. **Local Values**: Defines a formatted Firestore instance name using a random identifier for uniqueness.

2. **Firestore Database Resource**:
   - Creates a Firestore Native database linked to the specified Google Cloud project.
   - Configures concurrency mode, app engine integration, deletion protection, and deletion policy.
   - Enables point-in-time recovery.

3. **Firestore Backup Schedule Resource**:
   - Sets up a daily recurring backup schedule with a retention period (defaulting to 7 days).
   - Links the backup schedule to the Firestore database instance.

## Usage Instructions

### Prerequisites

- Install Terraform on your local machine.
- Configure Google Cloud SDK and authenticate your account.
- Ensure your account has sufficient IAM permissions to create Firestore databases and backup schedules.
- Enable required Google Cloud services (Firestore API, Backup API) in the project.

### Module Configuration

1. **Define Variables**: Create a `variables.tf` file including the following variables (example):

   ```hcl
   variable "create_firestore" {
     description = "Flag to create the Firestore database and backup schedule."
     type        = bool
     default     = true
   }

   variable "region" {
     description = "Google Cloud region where the Firestore instance will be created."
     type        = string
   }

2. **Local Project Info**: The module expects a `local.project.project_id` variable representing the target Google Cloud project ID. Define this appropriately in your configuration or pass it as an input.

3. **Initialize Terraform**:

   ```bash
   terraform init
   ```

4. **Plan the Deployment**:

   ```bash
   terraform plan -var-file="your_variables_file.tfvars"
   ```

5. **Apply the Configuration**:

   ```bash
   terraform apply -var-file="your_variables_file.tfvars"
   ```

### Example Usage

Here’s an example of calling this module in your Terraform configuration:

```hcl
module "firestore_setup" {
  source           = "./path_to_module"
  create_firestore = true
  region           = "us-central1"

  # Ensure local.project.project_id is set elsewhere in your config or passed in
}
```

## Outputs

This module currently does not define any outputs. You may consider adding outputs such as:

* Firestore database ID or name
* Backup schedule ID
* Project ID

## Conclusion

This Terraform module streamlines the deployment and management of Google Cloud Firestore databases with a built-in daily backup schedule. By using this module, users can ensure that their Firestore data is both highly available and protected against data loss through automated backups and point-in-time recovery. Following the usage instructions allows quick and consistent infrastructure provisioning in a repeatable manner.

# End User Documentation for Terraform GKE Cluster Module

## Overview

This Terraform module automates the creation and configuration of a Google Kubernetes Engine (GKE) private cluster with a preemptible node pool. It also configures the Kubernetes provider to interact with the newly created cluster using dynamically fetched credentials. The module enables features like workload identity, logging, monitoring, autoscaling, and cost management to provide a secure, scalable, and cost-efficient Kubernetes environment on Google Cloud.

## Features

- **GKE Private Cluster Creation**: Creates a private GKE cluster with customizable network and regional settings.
- **Node Pool with Preemptible Nodes**: Configures a preemptible node pool to optimize costs while maintaining performance.
- **Kubernetes Provider Configuration**: Automatically sets up the Kubernetes provider using cluster credentials for resource provisioning.
- **Addons Enabled**: Includes HTTP Load Balancing, Horizontal Pod Autoscaling, and GCS Fuse CSI driver.
- **Security and Monitoring**: Enables workload identity, basic security posture, logging, and managed Prometheus monitoring.
- **Release Channel and Cost Management**: Uses the REGULAR release channel and enables GKE cost management features.

## Module Structure

The module consists of the following components:

1. **Data Sources**:
   - `google_client_config.gke_name`: Fetches client authentication info.
   - `google_container_cluster.gke_name`: Reads the created GKE cluster details after provisioning.

2. **Local Variables**:
   - `k8s_credentials_cmd`: Stores a convenient `gcloud` CLI command to fetch cluster credentials manually if needed.

3. **GKE Cluster Resource**:
   - Creates a private GKE cluster with customizable options such as network, location, node pool removal, and add-ons.
   - Configures workload identity, security posture, logging, monitoring, and release channel.

4. **Kubernetes Provider Configuration**:
   - Uses the cluster endpoint, access token, and CA certificate to configure the Kubernetes provider for managing resources on the cluster.

5. **Node Pool Resource**:
   - Creates a preemptible node pool with specified machine types and node count.
   - Assigns a dedicated service account and scopes for node operations.

## Usage Instructions

### Prerequisites

- Terraform installed locally.
- Google Cloud SDK configured and authenticated.
- IAM permissions to create GKE clusters, node pools, and service accounts.
- A VPC network module or existing network named accordingly to provide `network_name` and subnet.

### Module Configuration

1. **Define Variables** (example):

   ```hcl
   variable "create_gke" {
     description = "Flag to create the GKE cluster and node pool."
     type        = bool
     default     = true
   }

   variable "gke_name" {
     description = "Name of the GKE cluster to create."
     type        = string
   }

2. **Local Variables**:

   * Ensure `local.project.project_id` and `local.region` are defined in your Terraform root module or passed accordingly.

3. **Initialize Terraform**:

   ```bash
   terraform init
   ```

4. **Plan the Deployment**:

   ```bash
   terraform plan -var-file="your_variables_file.tfvars"
   ```

5. **Apply the Configuration**:

   ```bash
   terraform apply -var-file="your_variables_file.tfvars"
   ```

### Example Usage

```hcl
module "gke_cluster" {
  source       = "./path_to_module"
  create_gke   = true
  gke_name     = "my-gke-cluster"

  # Ensure local.project.project_id and local.region are set in your root module or via locals
}
```

### Accessing the Cluster

To authenticate your local `kubectl` client, run:

```bash
gcloud container clusters get-credentials my-gke-cluster --region <region> --project <project-id>
```

Alternatively, the module sets up the Kubernetes provider with credentials for Terraform-managed resources.

## Outputs

This module does not define explicit outputs by default. Consider adding outputs such as:

* `cluster_name`
* `cluster_endpoint`
* `node_pool_name`
* `kubeconfig` (if generating one)

## Conclusion

This Terraform module provides a comprehensive solution to deploy a fully configured GKE private cluster with a cost-effective preemptible node pool and essential Kubernetes integrations. It facilitates secure and scalable Kubernetes infrastructure on Google Cloud with built-in support for monitoring, logging, autoscaling, and workload identity, simplifying cluster management and operational efficiency.


# End User Documentation for Terraform Google Cloud IAM Role Bindings Module

## Overview

This Terraform module manages IAM role bindings for a Google Cloud project by assigning predefined roles to trusted users and groups. It facilitates fine-grained access control by granting different roles—such as Viewer, Compute Instance Admin, Cloud Run Developer, Cloud SQL Client, Storage Object Admin, Secret Manager Accessor, and more—based on configurable flags and input user/group lists. The module also configures IAM bindings for Identity-Aware Proxy (IAP) access and billing notification permissions.

## Features

- **Role-Based Access Control (RBAC)**: Assigns multiple Google Cloud IAM roles to trusted users and groups in a scalable, repeatable way.
- **Dynamic Membership**: Uses `for_each` on sets derived from user and group email lists, ensuring unique IAM bindings.
- **Conditional Role Assignment**: Roles like Cloud Run Developer, Cloud SQL Client, Storage Object Admin, and IAP access are created based on module variables, enabling flexible deployments.
- **IAP Role Bindings**: Configures IAM bindings specifically for Google Cloud IAP with a wait period to ensure services are enabled.
- **Billing Notification Access**: Grants billing budget notification access to a specified email.
- **Owner Role Management**: Separately manages assignment of the Owner role to specified users and groups.

## Module Structure

The module is composed of these key resource groups:

1. **IAM Bindings for Standard Roles**  
   Resources of type `google_project_iam_member` assign the following roles to trusted users and groups:  
   - Viewer (`roles/viewer`)  
   - Compute Instance Admin (`roles/compute.viewer`)  
   - Cloud Run Developer (`roles/run.viewer`)  
   - Cloud SQL Client (`roles/cloudsql.client`)  
   - Cloud SQL Studio User (`roles/cloudsql.studioUser`)  
   - Storage Object Admin (`roles/storage.objectAdmin`)  
   - Secret Manager Secret Accessor (`roles/secretmanager.secretAccessor`)  
   - Logs Viewer (`roles/logging.viewer`)  
   - Monitoring Viewer (`roles/monitoring.viewer`)

2. **IAP IAM Bindings**  
   - A `time_sleep` resource delays IAM binding creation for 120 seconds to allow for service enablement.  
   - `google_iap_web_iam_member` binds IAP access roles to specified owner users/groups.  
   - A billing notification user is granted IAP access.

3. **Owner Role Bindings**  
   - IAM bindings for Owner role (`roles/owner`) are assigned to owner users and groups.

## Usage Instructions

### Prerequisites

- Terraform installed and configured.
- Google Cloud SDK authenticated with sufficient permissions to manage IAM roles on the project.
- The project must have required APIs enabled before IAP role bindings can be created.
- Prepare the lists of trusted and owner users and groups in email format.

### Required Variables (examples)

Define variables to control user/group lists and feature flags:

```hcl
variable "trusted_users" {
  type        = list(string)
  description = "List of trusted user email addresses."
}

variable "trusted_groups" {
  type        = list(string)
  description = "List of trusted group email addresses."
}

variable "owner_users" {
  type        = list(string)
  description = "List of owner user email addresses."
}

variable "owner_groups" {
  type        = list(string)
  description = "List of owner group email addresses."
}

variable "create_cloudrun" {
  type        = bool
  default     = false
  description = "Flag to create Cloud Run Developer IAM bindings."
}

variable "create_database" {
  type        = bool
  default     = false
  description = "Flag to create Cloud SQL Client and Studio User IAM bindings."
}

variable "create_storage" {
  type        = bool
  default     = false
  description = "Flag to create Storage Object Admin IAM bindings."
}

variable "create_iap" {
  type        = bool
  default     = false
  description = "Flag to create IAP IAM bindings."
}

variable "billing_budget_notification_email_addresses" {
  type        = list(string)
  description = "List of email addresses for billing budget notifications."
}
````

### Initialize, Plan, and Apply

1. Initialize Terraform:

```bash
terraform init
```

2. Review plan:

```bash
terraform plan -var-file="your_variables_file.tfvars"
```

3. Apply changes:

```bash
terraform apply -var-file="your_variables_file.tfvars"
```

## Example Usage

```hcl
module "iam_bindings" {
  source = "./path_to_module"

  trusted_users = [
    "alice@example.com",
    "bob@example.com",
  ]

  trusted_groups = [
    "devs@example.com",
  ]

  owner_users = [
    "owner@example.com",
  ]

  owner_groups = [
    "admins@example.com",
  ]

  create_cloudrun = true
  create_database = true
  create_storage  = true
  create_iap      = true

  billing_budget_notification_email_addresses = [
    "billing-notify@example.com",
  ]
}
```

## Notes

* The module ensures uniqueness of IAM bindings by using sets constructed from concatenated user and group lists.
* The `time_sleep` resource waits 120 seconds after enabling services before creating IAP IAM bindings to avoid race conditions.
* Roles such as Cloud Run Developer, Cloud SQL Client, and Storage Object Admin are conditionally created based on feature flags.
* Be cautious with Owner role assignments as they grant full project control.

## Conclusion

This Terraform module provides a flexible and scalable way to manage Google Cloud IAM roles for trusted users and groups. By centralizing IAM bindings with conditional logic, it simplifies project access control, enhances security posture, and streamlines permission management across multiple Google Cloud services including Compute, Cloud Run, Cloud SQL, Storage, Secret Manager, Logging, Monitoring, and IAP.

# End User Documentation for Terraform Google IAP Brand and Client Module

## Overview

This Terraform module automates the creation and management of Google Cloud Identity-Aware Proxy (IAP) OAuth brand and OAuth client for a project. It uses an external data source to check if the IAP OAuth brand already exists, avoiding duplicate creation. When enabled, the module creates a new IAP brand and associated OAuth client to support authentication for IAP-protected applications.

## Features

- **Brand Existence Check**: Uses an external script to verify if the OAuth brand titled "Cloud IAP Protected Application" already exists in the project.
- **Conditional Resource Creation**: Creates the IAP brand and OAuth client only if the brand does not exist and the feature flag is enabled.
- **Support Email Configuration**: Allows specification of a support email for the IAP brand.
- **Dependency Management**: Ensures correct creation order by depending on a timed delay resource (`time_sleep`) to avoid race conditions.

## Module Structure

The module consists of the following components:

1. **External Data Source (`data.external.check_brand`)**  
   Runs a Bash script invoking `gcloud` CLI to query existing IAP OAuth brands in the project.  
   Returns `"true"` if the brand `"Cloud IAP Protected Application"` exists, otherwise `"false"`.

2. **IAP Brand Resource (`google_iap_brand.project_brand`)**  
   Creates a new IAP OAuth brand with the title `"Cloud IAP Protected Application"` and a specified support email, but only if the brand does not already exist and the `create_iap` flag is true.

3. **IAP Client Resource (`google_iap_client.project_client`)**  
   Creates an OAuth client associated with the newly created IAP brand, used for configuring OAuth consent screens and client IDs.

## Usage Instructions

### Prerequisites

- Terraform installed and configured.
- Google Cloud SDK installed and authenticated on the machine running Terraform.
- `gcloud` CLI must have permissions to list IAP OAuth brands.
- The project must be enabled for IAP.
- A `time_sleep` resource or equivalent mechanism in your Terraform setup to delay resource creation and avoid API race conditions.

### Required Variables

```hcl
variable "create_iap" {
  description = "Flag to enable creation of IAP brand and client."
  type        = bool
  default     = false
}

variable "support_email" {
  description = "Support email address used in the IAP OAuth brand."
  type        = string
}
````

### Example Usage

```hcl
module "iap_brand_client" {
  source       = "./path_to_module"
  create_iap   = true
  support_email = "support@example.com"

  # Ensure local.project.project_id and time_sleep.wait_120_seconds exist in your root module
}
```

### Workflow

1. The external data source executes a shell command using `gcloud` to check if the brand exists.
2. If the brand does not exist and `create_iap` is true, the module creates the brand.
3. Then, it creates an OAuth client linked to the brand.
4. Both resource creations depend on a 120-second delay (`time_sleep.wait_120_seconds`) to ensure the GCP APIs are ready.

## Notes

* The module uses the fixed application title `"Cloud IAP Protected Application"`.
* The external data source relies on the `gcloud` CLI being installed and properly authenticated.
* The `time_sleep.wait_120_seconds` resource must be defined externally to delay resource creation (to avoid race conditions).
* If the brand already exists, the module will skip creating the brand and client resources.

## Conclusion

This Terraform module provides an automated, idempotent method for managing Google Cloud IAP OAuth brands and clients, essential for securing applications behind IAP. By checking for existing brands and conditionally creating new ones, it prevents duplication and ensures smooth integration with Google Cloud's IAP services.

# End User Documentation for Terraform Google Cloud Redis Instance Module

## Overview

This Terraform module provisions a Google Cloud Memorystore for Redis instance with a basic configuration. It enables users to create a single-node Redis instance with persistence, scheduled maintenance, and private network connectivity within a specified Google Cloud region. The module is designed for simple, cost-effective Redis deployment with controlled availability and backup policies.

## Features

- **Redis Instance Creation**: Automatically creates a Google Cloud Redis instance with a unique name.
- **Basic Tier**: Uses the BASIC tier suitable for development or small workloads.
- **Memory Size**: Configured with 1 GB of memory.
- **Redis Version**: Deploys Redis version 6.x.
- **Regional Deployment**: Creates the instance in a specified region and zone.
- **Network Configuration**: Connects Redis via direct peering to a specified VPC network.
- **Maintenance Policy**: Schedules weekly maintenance windows to minimize disruption.
- **Persistence Configuration**: Enables RDB snapshot persistence with daily snapshots.
- **Lifecycle Management**: Allows resource destruction (no prevent_destroy).

## Module Structure

The module includes these primary components:

1. **Local Variables**  
   - Defines a unique Redis instance name using a random ID.

2. **Redis Instance Resource (`google_redis_instance.redis_instance`)**  
   - Conditionally creates the Redis instance based on the `create_redis` flag.  
   - Configures instance properties such as tier, memory size, region, Redis version, and location.  
   - Sets up network authorization and connect mode to integrate with a VPC network module.  
   - Defines a weekly maintenance window on Sundays at 00:30.  
   - Configures persistence using RDB snapshots every 24 hours.  
   - Manages lifecycle with no destruction prevention.  
   - Includes dependencies on project setup, VPC, and enabled Google Cloud services.

## Usage Instructions

### Prerequisites

- Terraform installed and configured.
- Google Cloud SDK authenticated with permissions to create Memorystore instances.
- A VPC network module or existing VPC with a network name for authorized access.
- The Google Cloud Memorystore API enabled for the project.

### Module Configuration

1. **Define Variables** (example):

```hcl
variable "create_redis" {
  description = "Flag to enable creation of the Redis instance."
  type        = bool
  default     = true
}

# Ensure `local.region` and `local.random_id` are defined in your root module or locals.
````

2. **Initialize Terraform**:

```bash
terraform init
```

3. **Plan the Deployment**:

```bash
terraform plan -var-file="your_variables_file.tfvars"
```

4. **Apply the Configuration**:

```bash
terraform apply -var-file="your_variables_file.tfvars"
```

### Example Usage

```hcl
module "redis_instance" {
  source       = "./path_to_module"
  create_redis = true

  # Define local.region and local.random_id in root or pass as inputs as needed.
}
```

## Outputs

This module does not currently define outputs. Consider adding outputs such as:

* Redis instance ID or name.
* Hostname or IP address for connection.
* Port number.

## Conclusion

This Terraform module simplifies provisioning a basic Google Cloud Redis instance with persistence and scheduled maintenance, connected securely within a VPC. It is ideal for developers and teams needing reliable caching or session storage with minimal configuration and operational overhead.

# End User Documentation for Terraform Google Cloud VPC Network Module

## Overview

This Terraform module automates the creation and configuration of a Google Cloud Virtual Private Cloud (VPC) network with subnets, firewall rules, Cloud NAT, and Private Service Connect setup. It provides a scalable, multi-region network foundation suitable for Google Compute Engine (GCE) and Google Kubernetes Engine (GKE) workloads with secure and controlled traffic flow.

## Features

- **Multi-Region Subnet Creation**: Creates two subnets (`gce-vpc-subnet` and `gke-vpc-subnet`) per specified region with private access enabled.
- **Custom Firewall Rules**: Defines ingress firewall rules to allow load balancer health checks, SSH via IAP, intra-VPC traffic, NFS, and HTTP/HTTPS traffic based on source ranges and target tags.
- **Cloud NAT with Cloud Router**: Creates a Cloud Router with NAT for outbound internet access from private subnets.
- **Private Service Connect Setup**: Allocates private IP address ranges and establishes service networking peering connections for private service access within the VPC.
- **Conditional Resource Creation**: Uses variables to conditionally create resources based on environment needs.

## Module Structure

1. **VPC Network Module (`terraform-google-modules/network/google`)**  
   - Creates the VPC network and regional subnets using provided CIDRs.  
   - Enables private Google access on subnets.

2. **Firewall Rules Module (`terraform-google-modules/network/google//modules/firewall-rules`)**  
   - Applies a set of custom firewall rules for secure, controlled ingress traffic.

3. **Cloud Router and Cloud NAT Module (`terraform-google-modules/cloud-router/google`)**  
   - Provisions a Cloud Router with a NAT gateway for internet egress from private subnets.

4. **Private Service Connect Configuration**  
   - Reserves a global internal IP address range for service networking peering.  
   - Creates a peering connection to enable Private Service Connect within the VPC.

## Usage Instructions

### Prerequisites

- Terraform installed and configured.
- Google Cloud SDK authenticated with permissions to create networks, firewall rules, routers, NATs, and service networking connections.
- Define or obtain subnet CIDR blocks for each region.
- Enable necessary Google Cloud APIs including Compute Engine, Service Networking, and Cloud Router.

### Required Variables (examples)

```hcl
variable "create_network" {
  description = "Flag to enable creation of network resources."
  type        = bool
  default     = true
}

variable "network_name" {
  description = "Name of the VPC network."
  type        = string
}

variable "regions" {
  description = "List of regions for subnet creation."
  type        = list(string)
}

variable "subnet_cidrs" {
  description = "Map of region to subnet CIDR blocks for gce and gke subnets."
  type = map(object({
    gce_subnet_cidr_range = string
    gke_subnet_cidr_range = string
  }))
}
````

### Initialize Terraform

```bash
terraform init
```

### Plan Deployment

```bash
terraform plan -var-file="your_variables_file.tfvars"
```

### Apply Configuration

```bash
terraform apply -var-file="your_variables_file.tfvars"
```

## Example Usage

```hcl
module "vpc" {
  source  = "./path_to_module"
  count   = var.create_network ? 1 : 0

  network_name = "my-vpc-network"
  regions      = ["us-central1", "us-west1"]
  subnet_cidrs = {
    "us-central1" = {
      gce_subnet_cidr_range = "10.0.1.0/24"
      gke_subnet_cidr_range = "10.0.2.0/24"
    },
    "us-west1" = {
      gce_subnet_cidr_range = "10.1.1.0/24"
      gke_subnet_cidr_range = "10.1.2.0/24"
    }
  }
}
```

## Notes

* Firewall rules include:

  * Allowing Google Cloud load balancer health checks on HTTP and NFS ports.
  * Allowing SSH connections through Identity-Aware Proxy (IAP).
  * Allowing unrestricted intra-VPC traffic between subnets.
  * Allowing NFS traffic to tagged NFS servers.
  * Allowing HTTP/S traffic to tagged HTTP servers.
* Cloud NAT is provisioned for all subnet IP ranges with BGP ASN 64514.
* Private Service Connect setup uses a /16 IP range reserved internally for service peering.
* All resources respect conditional creation based on the `create_network` variable.

## Outputs

This module currently does not provide outputs. You may consider adding:

* VPC network name or ID.
* Subnet names and CIDRs.
* Cloud NAT gateway details.
* Private Service Connect peering info.

## Conclusion

This module offers a comprehensive, production-ready VPC setup for Google Cloud projects. It handles network provisioning, security through firewall rules, outbound internet access via Cloud NAT, and private connectivity with Private Service Connect. This foundation supports a wide range of GCE and GKE workloads with scalability and security best practices.

# End User Documentation for Terraform Google Cloud NFS Server Module

## Overview

This Terraform module provisions a Google Cloud NFS server setup including:

- A service account with Storage Admin role
- Static internal IP reservation for NFS server VM
- Instance template and managed instance group for NFS server VM with persistent SSD data disk and startup script
- TCP health check on NFS port
- Auto-healing and update policies for high availability
- Daily snapshot schedule for data disk backups

The module is designed to deploy a robust, stateful NFS service running on Google Compute Engine within a secured VPC network.

## Features

- **Service Account with Storage Admin Role** for accessing Cloud Storage
- **Static Internal IP Address** allocated for the NFS server VM
- **Instance Template** using Ubuntu 22.04 LTS, preconfigured with startup script
- **Persistent SSD Disk** attached for NFS storage with snapshot-based backups
- **Managed Instance Group** (MIG) with one instance, supporting auto-healing and rolling updates
- **Health Checks** on standard NFS port (2049)
- **Resource Policy** for daily automated snapshots with 7-day retention
- **Integration with VPC network and appropriate dependencies**

## Module Structure

### IAM Binding

- `google_project_iam_member.nfs_server_storage_admin`: Assigns `roles/storage.admin` to the NFS server service account.

### Networking

- `google_compute_address.static_internal_ip`: Reserves an internal static IP for the NFS server VM in the specified subnet.

### Compute Resources

- `google_compute_instance_template.nfs_server`: Defines the VM template including disks, machine type, network, service account, and startup script.
- `google_compute_instance_group_manager.nfs_server`: Manages the instance group of NFS server VM(s), supporting stateful disks and named ports.
- `google_compute_health_check.nfs_server_health_check`: TCP health check on port 2049 to monitor VM health.

### Snapshots

- `google_compute_resource_policy.daily_snapshot`: Defines a daily snapshot schedule for the data disk with retention and deletion policies.

## Usage Instructions

### Prerequisites

- Terraform installed and configured.
- Google Cloud SDK authenticated with sufficient IAM permissions.
- A VPC network with a subnet named `gce-vpc-subnet`.
- The startup script `create_nfs.sh` available in the module `scripts` directory.
- Enable Compute Engine and related APIs on the project.
- Define the required variables such as `create_nfs`, `nfs_machine`, `nfs_capacity`, `local.region`, and `local.project.project_id`.

### Variables (example)

```hcl
variable "create_nfs" {
  description = "Flag to enable creation of the NFS server resources."
  type        = bool
  default     = true
}

variable "nfs_machine" {
  description = "Machine type for the NFS server VM."
  type        = string
  default     = "e2-standard-2"
}

variable "nfs_capacity" {
  description = "Size in GB for the persistent SSD data disk."
  type        = number
  default     = 100
}
````

### Initialize Terraform

```bash
terraform init
```

### Plan Deployment

```bash
terraform plan -var-file="your_variables_file.tfvars"
```

### Apply Configuration

```bash
terraform apply -var-file="your_variables_file.tfvars"
```

## Example Usage

```hcl
module "nfs_server" {
  source       = "./path_to_module"
  create_nfs   = true
  nfs_machine  = "e2-standard-2"
  nfs_capacity = 200

  # local.project.project_id and local.region must be defined in your root or passed as locals
}
```

## Notes

* The VM boots from Ubuntu 22.04 LTS with an attached persistent SSD for NFS data.
* The startup script (`create_nfs.sh`) configures NFS server services.
* The managed instance group ensures high availability and self-healing.
* Named ports expose NFS (2049) and RPC bind (111) services.
* Daily snapshots are retained for 7 days and kept even if the disk is deleted.
* Dependencies are set to ensure correct creation order, including a 120-second wait after VPC setup.

## Outputs

Consider adding outputs for:

* NFS server IP address
* Instance group name
* Snapshot schedule policy ID

## Conclusion

This Terraform module provides a production-ready NFS server deployment on Google Cloud with automated backups, health checks, and high availability. It simplifies managing persistent, shared storage for applications requiring NFS access within a secure and scalable infrastructure.

# End User Documentation for Terraform Google Cloud Organization Policies Module

## Overview

This Terraform module manages several Google Cloud organization policies at the project level, allowing fine-grained governance over key project settings. It supports conditional application of policies related to Shielded VM enforcement, uniform bucket-level access for Cloud Storage, and domain-restricted IAM sharing. The module also incorporates a timed delay to ensure that policies and services are fully applied before subsequent operations proceed.

## Features

- **Shielded VM Policy Management**  
  Controls the enforcement of Shielded VMs within the project to enhance VM security posture.

- **Uniform Bucket-Level Access Policy**  
  Configures Cloud Storage bucket-level access uniformity, which impacts access control consistency.

- **Domain-Restricted Sharing Policy**  
  Restricts or allows IAM policy member domains for sharing, useful for controlling cross-domain access.

- **Timed Delay Resource**  
  Introduces a 120-second wait after applying policies and enabling services to mitigate eventual consistency issues.

## Module Structure

1. **Shielded VM Organization Policy (`google_project_organization_policy.shielded_vm_policy`)**  
   - Applies the `compute.requireShieldedVm` constraint with enforcement disabled (allowing both shielded and unshielded VMs).  
   - Created only if `set_shielded_vm_policy` is `true`.

2. **Bucket-Level Access Organization Policy (`google_project_organization_policy.bucket_level_access_policy`)**  
   - Applies the `constraints/storage.uniformBucketLevelAccess` constraint with enforcement disabled (allowing non-uniform bucket-level access).  
   - Created only if `set_bucket_level_access_policy` is `true`.

3. **Domain-Restricted Sharing Policy (`google_project_organization_policy.domain_restricted_sharing_policy`)**  
   - Applies the `iam.allowedPolicyMemberDomains` constraint to allow sharing with all domains.  
   - Created conditionally when `set_domain_restricted_sharing_policy`, `create_budget`, and `billing_budget_pubsub_topic` are all `true`.

4. **Timed Delay Resource (`time_sleep.wait_120_seconds`)**  
   - Waits 120 seconds to ensure policies and enabled services have taken effect before continuing.  
   - Created conditionally based on any of the policy flags or service enabling.

## Usage Instructions

### Prerequisites

- Terraform installed and configured.
- Google Cloud SDK authenticated with permissions to modify project organization policies.
- The target project must exist and be referenced via `local.project.project_id`.
- The `module.gcp_project` must be defined and applied before these policies.

### Variables (example)

```hcl
variable "set_shielded_vm_policy" {
  description = "Enable setting Shielded VM policy."
  type        = bool
  default     = false
}

variable "set_bucket_level_access_policy" {
  description = "Enable setting Bucket Level Access policy."
  type        = bool
  default     = false
}

variable "set_domain_restricted_sharing_policy" {
  description = "Enable setting domain-restricted sharing policy."
  type        = bool
  default     = false
}

variable "create_budget" {
  description = "Flag indicating whether a billing budget is created."
  type        = bool
  default     = false
}

variable "billing_budget_pubsub_topic" {
  description = "Flag indicating if Pub/Sub topic for billing budget notifications exists."
  type        = bool
  default     = false
}

variable "enable_services" {
  description = "Flag indicating whether Google Cloud services are enabled."
  type        = bool
  default     = false
}
````

### Initialize Terraform

```bash
terraform init
```

### Plan Deployment

```bash
terraform plan -var-file="your_variables_file.tfvars"
```

### Apply Configuration

```bash
terraform apply -var-file="your_variables_file.tfvars"
```

## Example Usage

```hcl
module "organization_policies" {
  source                           = "./path_to_module"
  set_shielded_vm_policy           = true
  set_bucket_level_access_policy   = true
  set_domain_restricted_sharing_policy = true
  create_budget                   = true
  billing_budget_pubsub_topic     = true
  enable_services                 = true

  # Ensure local.project.project_id and module.gcp_project are properly configured
}
```

## Notes

* The Shielded VM and Bucket Level Access policies are configured with enforcement disabled to allow backward compatibility.
* The domain-restricted sharing policy, when enabled, allows sharing with all domains.
* The 120-second wait resource ensures consistent application of organization policies before dependent resources are created or modified.
* All resources depend on the successful creation of the referenced GCP project module.

## Conclusion

This module provides a controlled way to manage critical organization policies for a Google Cloud project. By enabling or disabling policies with clear flags and ensuring proper sequencing with a timed wait, it helps maintain project governance while minimizing disruptions caused by eventual consistency in policy enforcement.

