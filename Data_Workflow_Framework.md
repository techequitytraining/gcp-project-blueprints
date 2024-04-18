# Data Workflow Framework

## Overview

This framework outlines a scalable and portable solution for data workflows, focusing on bioinformatics as a use case, utilizing Nextflow, a powerful workflow manager designed for complex computational pipelines in the life sciences domain. Nextflow facilitates the development of portable and reproducible workflows, capable of running across various execution platforms such as local environments, high-performance computing schedulers, and cloud-based services including Google Cloud Life Sciences and Kubernetes. It also integrates seamlessly with dependency management tools like Conda and Docker, enhancing the reproducibility and portability of computational analyses.

## Features

- **Portability and Reproducibility:** Nextflow supports a variety of execution platforms and dependency management systems, ensuring workflows can be easily shared and reproduced.
- **Secure Workflow Execution:** The framework deploys a Nextflow server alongside necessary configurations, including a Service Account and firewall rules, to facilitate secure access through an Identity-Aware Proxy Tunnel, eliminating the need for public IP addresses.
- **Automated Resource Provisioning:** Upon deployment, resources such as a Cloud Storage Bucket for workflow execution are automatically created, streamlining the setup process.
- **Ease of Access:** Secure Shell access to the Nextflow server is simplified, enabling straightforward interaction with the deployed environment.

## Cloud Services Utilized

- Life Sciences API and Batch API for workflow execution
- Cloud Compute for server hosting
- Cloud Storage for data management
- Virtual Private Cloud for network security
- Billing Budget for cost management

## Reference Architecture

The diagram below illustrates the foundational architecture of the bioinformatics workflow framework, showcasing the components and their interactions within the cloud environment. This architecture is designed to be modular and adaptable to specific research needs.

![](./images/architecture.png)

## Getting Started

- **Initial Setup:** Once deployed, a storage bucket is created for workflow execution. Access to non-public input files requires configuration of the Service Account.
- **Accessing the Server:** Secure Shell access to the Nextflow server is provided, with detailed instructions for initial login and troubleshooting.
- **Workflow Execution:** Examples are provided to test the deployment with both Life Sciences API and Batch API, ensuring a smooth start with the framework.

## Additional Information

Nextflow's documentation offers extensive guidance on developing and deploying workflows, available at [Nextflow Documentation](https://www.nextflow.io/docs/latest/). This framework serves as a starting point for leveraging Nextflow within a cloud environment, providing a scalable and secure platform for bioinformatics research.
