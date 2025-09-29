# End-User Documentation

## 1. Introduction and Getting Started

Welcome to the Rapid Application Deployment documentation. This document will guide you through key features and functionalities of the platform.

### 1.1. Logging In

Access to the application is handled through a secure sign-on system. Upon your first visit, you will be prompted to log in with your Google credentials.

### 1.2. The Main Dashboard (Deployments Page)

After logging in, you will land on the **Deployments** page. This is your main dashboard, providing a comprehensive overview of all module deployments.

- **For Standard Users:** You will see a list of all the modules you have personally deployed ("My Deployments").
- **For Administrators:** You will have access to two tabs:
    - **My Deployments:** Shows the modules you have deployed.
    - **All Deployments:** Shows a list of every module deployed by all users across the platform.

### 1.3. Navigating the Dashboard

- **Search:** A search bar is located at the top of the page, allowing you to quickly find specific deployments. You can search by a module's name, its deployment ID, or the associated project ID. Administrators can also search by the email of the user who initiated the deployment.
- **Create New Deployment:** To deploy a new module, click the **Create New** button. This will take you to the module selection screen.
- **Deployment List:** The main area of the page lists your deployments with key information such as the module name, status, creation date, and associated project ID. You can click on any deployment to view more details.
- **Pagination:** If there are many deployments, you can navigate through the pages using the controls at the bottom of the list.

## 2. Deploying a Module

The core functionality of the application is to deploy pre-built modules. This process is designed to be straightforward.

### 2.1. Selecting a Module

1. From the **Deployments** page, click the **Create New** button.
2. You will be taken to the module selection page, which is divided into two categories:
    - **Platform Modules:** These are standard, globally available modules.
    - **Partner Modules:** If you are a partner and have configured your GitHub repository in your profile, you will see this tab. It contains modules specific to your organization.
3. Browse the available modules. Each module card displays its name, a brief description, and the credit cost to deploy it.
4. Click on the card of the module you wish to deploy.

### 2.2. Configuring the Deployment

After selecting a module, you will be taken to the provisioning page, where you need to configure the deployment.

1. **Configuration Form:** A form will be displayed with a series of fields. These are the variables required to deploy the module, such as project IDs, regions, or other specific settings.
2. **Fill out the Form:** Complete all the required fields with the appropriate information for your deployment.
3. **Submit:** Once you have filled out the form, click the **Submit** button. The application will display module dependencies and validate your inputs and, if your credit balance is sufficient, begin the deployment process.

You will be redirected back to the **Deployments** page, where you can monitor the status of your new deployment.

## 3. Billing (for Users)

The **Billing** page allows you to manage your credits, subscriptions, and view your spending history. You can access it from the main navigation menu.

### 3.1. Subscription Tiers

This tab displays all available subscription tiers. You can subscribe to a tier to receive a recurring amount of credits.

- **Subscribing:** Click the **Subscribe** button on your desired tier to be redirected to a secure payment page.
- **Active Subscriptions:** If you have an active subscription, it will be highlighted, and the other tiers will be disabled.
- **Managing Subscriptions:** You can manage your active subscription, such as canceling the auto-renewal, from this tab.

### 3.2. Credit Transactions

This tab provides a detailed history of all your credit transactions.

- **Transaction History:** The table lists all credit additions (from subscriptions or direct purchases) and deductions (from deploying modules or project costs). It shows the date, category, amount, and your resulting credit balance after each transaction.
- **Search and Filter:** You can search for specific transactions by deployment ID or filter the list by date.
- **Export:** You can export your credit history as a CSV file by clicking the **Export CSV** button.

### 3.3. Project Costs

This tab shows you the ongoing costs associated with your deployed projects, deducted from your credit balance. It helps you monitor your spending over time.

### 3.4. Monthly Invoices

Here you can view and download your monthly invoices for your records.

### 3.5. Send Message

This section provides forms for contacting support or inviting new users to the platform.

## 4. Billing (for Administrators)

Administrators have an expanded view of the **Billing** page, with additional tabs for managing the platform's finances and users.

### 4.1. Subscription Tiers Management

This tab allows administrators to create, edit, and manage the subscription tiers offered to users. You can define the price, the number of credits included, and the billing period for each tier.

### 4.2. User Credits

This tab provides a list of all users on the platform. Administrators can:

- **View User Information:** See each user's email, credit balance, and partner status.
- **Edit User Details:** Click the **Edit** button next to a user to:
    - **Adjust Credit Balance:** Manually add or remove credits from a user's account.
    - **Change Partner Status:** Grant or revoke partner status for a user.
    - **Activate/Deactivate Account:** Toggle a user's account status.
- **Search:** Find specific users by searching for their email address.

### 4.3. Project Costs and Revenue

- **Project Costs:** This tab provides an aggregated view of the costs incurred by all projects across the platform.
- **Project Revenue:** This tab shows the total revenue generated from project deployments, giving administrators insight into the platform's financial performance.

### 4.4. Credit Settings

This page contains forms for awarding credits to users, either individually or in bulk, and for managing other credit-related settings.

## 5. Managing Your Profile

The **Profile** page allows you to manage your personal information and settings.

### 5.1. Profile Information

This section displays your basic profile information, including your name, email address, and profile picture, as provided by the authentication system.

### 5.2. Email Notification Settings

You can control which email notifications you receive from the application. You can toggle notifications for:

- **Deployments:** Receive updates on the status of your module deployments.
- **Billing:** Receive notifications related to your subscriptions and credit balance.

### 5.3. Partner Settings

If you are a partner user, this section allows you to connect your GitHub account to the platform.

1.  **GitHub Token:** Provide a GitHub personal access token with the necessary permissions to read your repositories.
2.  **GitHub Repository:** Once the token is saved, you can select the specific repository that contains your partner modules. This will populate the **Partner Modules** tab on the module deployment page.

### 5.4. Deleting Your Account

At the bottom of the page, you will find an option to permanently delete your account. This action is irreversible and will remove all your data from the platform.

## 6. Administration

Administrators have access to a special **Admin** page for configuring global settings.

### 6.1. Admin Settings

This page allows administrators to set up the foundational configurations for the platform. This includes:

- **Platform GitHub Integration:** Similar to the partner settings, administrators must provide a GitHub token and select a repository. This repository is used to source the **Platform Modules** that are available to all users.
- **Mailbox Credentials:** Configure the credentials for the email service used to send notifications from the platform.
- **Other Global Variables:** Set other system-wide variables that control the application's behavior.

---

This concludes the end-user documentation. For technical details or developer information, please refer to the project's README file or contact the development team.
