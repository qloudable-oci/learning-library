# Using Events and Notification

## Table of Contents

[Overview](#overview)

[Pre-Requisites](#pre-requisites)

[Sign in to OCI Console and configure Notification and Event](#sign-in-to-oci-console-and-configure-notification-and-event)

[Create VCN](#create-vcn)

[Create compute instance and verify notification](#create-compute-instance-and-verify-notification)

[Delete the resources](#delete-the-resources)

## Overview

The Oracle Cloud Infrastructure Notifications service broadcasts messages  to distributed components through a publish-subscribe pattern, delivering secure, highly reliable, low latency and durable messages for applications hosted on Oracle Cloud Infrastructure and externally.
The Notifications service enables you to set up communication channels for publishing messages using topics  and subscriptions . When a message is published to a topic, the Notifications service sends the message to all of the topic's subscriptions.
In this lab we will verify notifications when a compute instance is launched and deleted

**Some Key points;**

**We recommend using Chrome or Edge as the broswer. Also set your browser zoom to 80%**

- All screen shots are examples ONLY. Screen shots can be enlarged by Clicking on them

- Login credentials are provided later in the guide (scroll down). Every User MUST keep these credentials handy.

- Do NOT use compartment name and other data from screen shots.Only use  data(including compartment name) provided in the content section of the lab

- Mac OS Users should use ctrl+C / ctrl+V to copy and paste inside the OCI Console

- Login credentials are provided later in the guide (scroll down). Every User MUST keep these credentials handy.

**Cloud Tenant Name**
**User Name**
**Password**
**Compartment Name (Provided Later)**

**Note:** OCI UI is being updated thus some screenshots in the instructions might be different than actual UI

## Pre-Requisites

1. OCI Training : https://cloud.oracle.com/en_US/iaas/training

2. Familiarity with OCI console: https://docs.us-phoenix-1.oraclecloud.com/Content/GSG/Concepts/console.htm

3. Overview of Networking: https://docs.us-phoenix-1.oraclecloud.com/Content/Network/Concepts/overview.htm

4. Familiarity with Compartments: https://docs.us-phoenix-1.oraclecloud.com/Content/GSG/Concepts/concepts.htm

5. Connecting to a compute instance: https://docs.us-phoenix-1.oraclecloud.com/Content/Compute/Tasks/accessinginstance.htm

## Sign in to OCI Console and configure Notification and Event

* **Tenant Name:** {{Cloud Tenant}}
* **User Name:** {{User Name}}
* **Password:** {{Password}}
* **Compartment:**{{Compartment}}

1. Sign in using your tenant name, user name and password. Use the login option under **Oracle Cloud Infrastructure**

<img src="https://qloudableassets.blob.core.windows.net/oci-learninglibrary/qloudable/Using_Events_and_Notification/img/1.png?st=2020-04-21T07%3A42%3A24Z&se=2023-04-22T07%3A42%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=odOiaDHfRVGu7lVnPrxtH5ZwsTH6su4eKpvpolyD7TE%3D" alt="image-alt-text">

2. First we will create a Notification topic and subscribe to this topic. From OCI Services menu,Click **Notifications** under **Application Integration**

3. Click **Create Topic** and fill out the dialog box:

- **Name**: Provide a name
- **Description**: Provide a description

3. Click **Create**

<img src="https://qloudableassets.blob.core.windows.net/oci-learninglibrary/qloudable/Using_Events_and_Notification/img/2.png?st=2020-04-21T07%3A42%3A24Z&se=2023-04-22T07%3A42%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=odOiaDHfRVGu7lVnPrxtH5ZwsTH6su4eKpvpolyD7TE%3D" alt="image-alt-text">

4. Once the topic state changes to **Active**, Click the topic Name. Click **Create Subscription** and fill out the dialog box:

- **PROTOCOL**: Email
- **EMAIL**: Provide your email id

5. Click **Create**

6. Subscription details screen will be displayed with subscription status showig **Pending**.

7. Check the email account you specifed and click the verification link for this subscription. Switch back to OCI console window and verify the subscription status chanegd to **Active**

8. You are now subsribed to a Notification topic. Next we will configure Events that will publish messages to this Notification topic.

9. From OCI Services menu,Click **Event Service** under **Application Integration**

10. Click **Create Rule**, Fill out the dialog box:

- **DISPLAY NAME** : Provide a name
- **DESCRIPTION** : Provide a description

Under **Rule Conditions**

- Ensure **Event Type** is selected
- **SERVICE NAME**: Compute
- **EVENT TYPE** : Choose these 4 Types from the drop down menu. **Instance-Launch Begin**, **Instance-Launch End**, **Instance-Terminate Begin**, **Instance-Terminate End**

Under **Actions**

- **ACTION TYPE**: Notifications
- **NOTIFICATIONS COMPARTMENT**: Choose your compartment 
- **TOPIC**: Choose the topic created earlier

11. Click **Create Rule**

<img src="https://qloudableassets.blob.core.windows.net/oci-learninglibrary/qloudable/Using_Events_and_Notification/img/3.png?st=2020-04-21T07%3A42%3A24Z&se=2023-04-22T07%3A42%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=odOiaDHfRVGu7lVnPrxtH5ZwsTH6su4eKpvpolyD7TE%3D" alt="image-alt-text">

**We have now configured Notification service and tied events to it with a specific compartment. When a new compute instance is launched or terminated an email notification will be sent to the email address specified**

## Create VCN 

1. From the OCI Services menu,Click **Virtual Cloud Network**. Select the compartment assigned to you from drop down menu on left part of the screen under Networking and Click **Start VCN Wizard**

<img src="https://qloudableassets.blob.core.windows.net/oci-learninglibrary/qloudable/Using_Events_and_Notification/img/4.png?st=2020-04-21T07%3A42%3A24Z&se=2023-04-22T07%3A42%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=odOiaDHfRVGu7lVnPrxtH5ZwsTH6su4eKpvpolyD7TE%3D" alt="image-alt-text">

<img src="https://qloudableassets.blob.core.windows.net/oci-learninglibrary/qloudable/Using_Custom_Image/img/v6.PNG?st=2020-04-17T13%3A29%3A35Z&se=2023-04-18T13%3A29%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=qiutPpXR23TPkCPGckbB84JaQFAwUef0NgOByiqtTXM%3D" alt="image-alt-text">

<img src="https://qloudableassets.blob.core.windows.net/oci-learninglibrary/qloudable/Using_Custom_Image/img/v1.PNG?st=2020-04-17T13%3A29%3A35Z&se=2023-04-18T13%3A29%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=qiutPpXR23TPkCPGckbB84JaQFAwUef0NgOByiqtTXM%3D" alt="image-alt-text">

**NOTE:** Ensure the correct Compartment is selected under COMPARTMENT list

3. Click **VCN with Internet Connectivity** and click **Start VCN Wizard**.

<img src="https://qloudableassets.blob.core.windows.net/oci-learninglibrary/qloudable/Using_Custom_Image/img/v2.PNG?st=2020-04-17T13%3A29%3A35Z&se=2023-04-18T13%3A29%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=qiutPpXR23TPkCPGckbB84JaQFAwUef0NgOByiqtTXM%3D" alt="image-alt-text">

4. Fill out the dialog box:

- **VCN NAME**: Provide a name
- **COMPARTMENT**: Ensure your compartment is selected
- **VCN CIDR BLOCK**: Provide a CIDR block (10.0.0.0/16)
- **PUBLIC SUBNET CIDR BLOCK**: Provide a CIDR block (10.0.1.0/24)
- **PRIVATE SUBNET CIDR BLOCK**: Provide a CIDR block (10.0.2.0/24)
- Click **Next**.

<img src="https://qloudableassets.blob.core.windows.net/oci-learninglibrary/qloudable/Using_Custom_Image/img/v3.PNG?st=2020-05-07T10%3A57%3A26Z&se=2023-05-08T10%3A57%3A00Z&sp=rl&sv=2018-03-28&sr=b&sig=5U%2BaJudLgu7GpmhEd6%2B6BabwjiZO84yNYbZ1%2Fn6JywY%3D" alt="image-alt-text">
<img src="https://qloudableassets.blob.core.windows.net/oci-learninglibrary/qloudable/Using_Custom_Image/img/v7.PNG?st=2020-05-07T10%3A58%3A17Z&se=2023-05-08T10%3A58%3A00Z&sp=rl&sv=2018-03-28&sr=b&sig=Tih%2FQGvqpJPJ7XIbuXlxch9YZh4qvHyzveAZkcvK9%2FU%3D" alt="image-alt-text">

5. Verify all the information and  Click **Create**.

<img src="https://qloudableassets.blob.core.windows.net/oci-learninglibrary/qloudable/Using_Custom_Image/img/v4.PNG?st=2020-04-17T13%3A29%3A35Z&se=2023-04-18T13%3A29%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=qiutPpXR23TPkCPGckbB84JaQFAwUef0NgOByiqtTXM%3D" alt="image-alt-text">

6. This will create a VCN with followig components.

**VCN**, **Public subnet**, **Private subnet**, **Internet gateway (IG)**, **NAT gateway (NAT)**, **Service gateway (SG)**

7. Click **View Virtual Cloud Network** to display your VCN details.

<img src="https://qloudableassets.blob.core.windows.net/oci-learninglibrary/qloudable/Using_Custom_Image/img/v5.PNG?st=2020-04-17T13%3A29%3A35Z&se=2023-04-18T13%3A29%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=qiutPpXR23TPkCPGckbB84JaQFAwUef0NgOByiqtTXM%3D" alt="image-alt-text">

## Create compute instance and verify notification

1. From OCI services menu, Click **Instances** under **Compute** 

2. Click **Create Instance**. Fill out the dialog box:

- **Name your instance**: Enter a name 
- **Choose an operating system or image source**: For the image, we recommend using the Latest Oracle Linux available.
- **Availability Domain**: Select availability domain
- **Instance Type**: Select Virtual Machine 
- **Instance Shape**: Select VM shape 

**Under Configure Networking**

- **Virtual cloud network compartment**: Select your compartment
- **Virtual cloud network**: Choose the VCN 
- **Subnet Compartment:** Choose your compartment. 
- **Subnet:** Choose the Public Subnet under **Public Subnets** 
- **Use network security groups to control traffic** : Leave un-checked
- **Assign a public IP address**: Check this option

<img src="https://qloudableassets.blob.core.windows.net/oci-learninglibrary/qloudable/Using_Events_and_Notification/img/5.png?st=2020-04-21T07%3A42%3A24Z&se=2023-04-22T07%3A42%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=odOiaDHfRVGu7lVnPrxtH5ZwsTH6su4eKpvpolyD7TE%3D" alt="image-alt-text">

- **Boot Volume:** Leave the default
- **Add SSH Keys:** Leave empty

3. Click **Create**

**NOTE:** If 'Service limit' error is displayed choose a different shape from VM.Standard2.1, VM.Standard.E2.1, VM.Standard1.1, VM.Standard.B1.1  OR choose a different AD

<img src="https://qloudableassets.blob.core.windows.net/oci-learninglibrary/qloudable/Using_Events_and_Notification/img/6.png?st=2020-04-21T07%3A42%3A24Z&se=2023-04-22T07%3A42%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=odOiaDHfRVGu7lVnPrxtH5ZwsTH6su4eKpvpolyD7TE%3D" alt="image-alt-text">

4. Switch to your email account and verify an event indicating compute instance launch was received

5. Wait for Instance to be in **Running** state. 

6. Switch to your email account and verify an event indicating compute instance create was received

## Delete the resources

1. Switch to  OCI console window

2. If your Compute instance is not displayed, From OCI services menu Click **Instances** under **Compute**

3. Locate first compute instance, Click Action icon and then **Terminat** 

<img src="https://qloudableassets.blob.core.windows.net/oci-learninglibrary/qloudable/Using_Events_and_Notification/img/7.png?st=2020-04-21T07%3A42%3A24Z&se=2023-04-22T07%3A42%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=odOiaDHfRVGu7lVnPrxtH5ZwsTH6su4eKpvpolyD7TE%3D" alt="image-alt-text">

4. Make sure Permanently delete the attached Boot Volume is checked, Click Terminate Instance. Wait for instance to fully Terminate

<img src="https://qloudableassets.blob.core.windows.net/oci-learninglibrary/qloudable/Using_Events_and_Notification/img/8.png?st=2020-04-21T07%3A42%3A24Z&se=2023-04-22T07%3A42%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=odOiaDHfRVGu7lVnPrxtH5ZwsTH6su4eKpvpolyD7TE%3D" alt="image-alt-text">

5. Switch to your email account and verify an event indicating compute instance terminate was received

6. Once the compute instance is fully terminted another email notification will arrive

7. From OCI services menu Click **Virtual Cloud Networks** under Networking, list of all VCNs will 
appear.

7. Locate your VCN , Click Action icon and then **Terminate**. Click **Delete All** in the Confirmation window. Click **Close** once VCN is deleted

<img src="https://qloudableassets.blob.core.windows.net/oci-learninglibrary/qloudable/Using_Events_and_Notification/img/9.png?st=2020-04-21T07%3A42%3A24Z&se=2023-04-22T07%3A42%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=odOiaDHfRVGu7lVnPrxtH5ZwsTH6su4eKpvpolyD7TE%3D" alt="image-alt-text">

8. Navigate to **Functions** under **Developer Servies**. Click the action icon and Click **Delete**

<img src="https://qloudableassets.blob.core.windows.net/oci-learninglibrary/qloudable/Using_Events_and_Notification/img/10.png?st=2020-04-21T07%3A42%3A24Z&se=2023-04-22T07%3A42%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=odOiaDHfRVGu7lVnPrxtH5ZwsTH6su4eKpvpolyD7TE%3D" alt="image-alt-text">

9. From OCI Services menu,Click **Notifications** under **Application Integration**. Click your Topic name

10. Click **Delete**

11. From OCI Services menu,Click **Event Service** under **Application Integration**

12. Click your Rule name and Click **Delete**. In the dialog box type **DELETE** and click **Delete**
 
***Congratulations! You have successfully completed the lab***
