# Scientific Data Analysis

## Overview

This blueprint details the setup of a powerful workflow management system designed specifically for scientific data analysis workflows. Utilizing Cromwell, an open-source Workflow Management System as a reference implementation, this blueprint provides a robust solution for managing, executing, and analyzing scientific workflows. Cromwell is recognized for its flexibility and is widely used in genomics research and other scientific fields. This platform simplifies the deployment and management of Cromwell servers, integrating them with essential cloud services to enhance security, scalability, and accessibility.

## Key Components

- **Cromwell:** An open-source Workflow Management System designed for scientific workflows, offering broad compatibility with various workflow languages. [Cromwell Documentation](https://cromwell.readthedocs.io/)
- **CloudSQL Instance:** Provides a managed database service to support Cromwell's backend, ensuring reliable data storage and management.
- **IAP Tunnel:** Enables secure access to the Cromwell server without the need for a public IP, safeguarding your workflows and data.

## Cloud Services Utilized

- **Life Sciences API:** Facilitates the execution of bioinformatics workflows in a scalable and cost-effective manner.
- **Cloud Compute:** Hosts the Cromwell server, providing compute resources for workflow execution.
- **CloudSQL:** Offers a managed relational database service for Cromwell's backend.
- **Cloud Storage:** Serves as a centralized storage solution for workflow inputs, outputs, and logs.
- **Virtual Private Cloud:** Ensures secure network management for cloud resources.
- **Billing Budget:** Monitors cloud spending to maintain cost efficiency.

## Reference Architecture

The architecture diagram below illustrates the comprehensive setup of the Scientific Data Analysis Workflow Platform. It highlights the integration of Cromwell with Google Cloud services, creating a secure and scalable environment for executing scientific workflows.

![](./images/architecture.png)

## Getting Started

- **Deploying Cromwell Server:** The deployment process automatically sets up a Cromwell server along with a CloudSQL instance. A firewall rule is added to enable secure access through an IAP Tunnel.
- **Workflow Execution:** Upon deployment, a Cloud Storage bucket is created for workflow execution. Users can submit jobs directly from their devices through a secure tunnel, leveraging the IAP Tunnel for both CLI and web UI access.
- **Accessing Cromwell Web UI:** The IAP Tunnel facilitates access to Cromwell's Swagger web UI, allowing users to interact with the API and access workflow timing graphs directly from a browser.

## Secure and Accessible

This platform ensures secure access to the Cromwell server and its web UI without exposing it to the public internet. Users can securely submit jobs and access workflow information from any device, enhancing the flexibility and security of scientific data analysis.

## Conclusion

The Scientific Data Analysis Workflow Platform Blueprint offers a streamlined approach to deploying and managing scientific workflows in the cloud. By leveraging Cromwell and integrating it with Google Cloud services, this platform provides a secure, scalable, and user-friendly environment for scientific research and data analysis.
