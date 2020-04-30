# Creating Compute Instance using Custom Image

## Table of Contents

[Overview](#overview)

[Pre-Requisites](#pre-requisites)

[Sign in to OCI Console and create VCN](#sign-in-to-oci-console-and-create-vcn)

[Create ssh keys and compute instance](#create-ssh-keys-and-compute-instance)

[Install httpd on compute instance and create custom image](#install-httpd-on-compute-instance-and-create-custom-image)

[Delete the resources](#delete-the-resources)

## Overview

In this lab we will use Custom image feature of OCI. Using this feature an existing Compute instance with software packages and updates installed can be used to created additional compute instance.  These new compute instances will come with all the software packages and updates pre-installed.

**Some Key points;**

**We recommend using Chrome or Edge as the broswer. Also set your browser zoom to 80%**

- All screen shots are examples ONLY. Screen shots can be enlarged by Clicking on them

- Login credentials are provided later in the guide (scroll down). Every User MUST keep these credentials handy.

- Do NOT use compartment name and other data from screen shots.Only use  data(including compartment name) provided in the content section of the lab

- Mac OS Users should use ctrl+C / ctrl+V to copy and paste inside the OCI Console

- Login credentials are provided later in the guide (scroll down). Every User MUST keep these credentials handy.

**Note:** OCI UI is being updated thus some screenshots in the instructions might be different than actual UI

## Pre-Requisites

1. Oracle Cloud Infrastructure account credentials (User, Password, Tenant, and Compartment)  

2. OCI Training : https://cloud.oracle.com/en_US/iaas/training

3. Familiarity with OCI console: https://docs.us-phoenix-1.oraclecloud.com/Content/GSG/Concepts/console.htm

4. Overview of Networking: https://docs.us-phoenix-1.oraclecloud.com/Content/Network/Concepts/overview.htm

5. Familiarity with Compartment: https://docs.us-phoenix-1.oraclecloud.com/Content/GSG/Concepts/concepts.htm

6. Connecting to a compute instance: https://docs.us-phoenix-1.oraclecloud.com/Content/Compute/Tasks/accessinginstance.htm

## Sign in to OCI Console and create VCN

* **Tenant Name:** {{Cloud Tenant}}
* **User Name:** {{User Name}}
* **Password:** {{Password}}
* **Compartment:**{{Compartment}}

1. Sign in using your tenant name, user name and password. Use the login option under **Oracle Cloud Infrastructure**

<img src="https://qloudableassets.blob.core.windows.net/oci-learninglibrary/qloudable/Using_Custom_Image/img/1.png?st=2020-04-17T13%3A29%3A35Z&se=2023-04-18T13%3A29%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=qiutPpXR23TPkCPGckbB84JaQFAwUef0NgOByiqtTXM%3D" alt="image-alt-text">

2. From the OCI Services menu,Click **Virtual Cloud Network**. Select the compartment assigned to you from drop down menu on left part of the screen under Networking and Click **Start VCN Wizard**

<img src="https://qloudableassets.blob.core.windows.net/oci-learninglibrary/qloudable/Using_Custom_Image/img/2.png?st=2020-04-17T13%3A29%3A35Z&se=2023-04-18T13%3A29%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=qiutPpXR23TPkCPGckbB84JaQFAwUef0NgOByiqtTXM%3D" alt="image-alt-text">

**NOTE:** Ensure the correct Compartment is selected under COMPARTMENT list

3. Click **VCN with Internet Connectivity** and click **Start VCN Wizard**

4. Fill out the dialog box:

- **VCN NAME**: Provide a name
- **COMPARTMENT**: Ensure your compartment is selected
- **VCN CIDR BLOCK**: Provide a CIDR block (10.0.0.0/16)
- **PUBLIC SUBNET CIDR BLOCK**: Provide a CIDR block (10.0.1.0/24)
- **PRIVATE SUBNET CIDR BLOCK**: Provide a CIDR block (10.0.2.0/24)
- Click **Next**

5. Verify all the information and  Click **Create**

6. This will create a VCN with followig components.

**VCN**, **Public subnet**, **Private subnet**, **Internet gateway (IG)**, **NAT gateway (NAT)**, **Service gateway (SG)**

7. Click **View Virtual Cloud Network** to display your VCN details.

## Create ssh keys and compute instance

1. Click the Apps icon in the toolbar and select  Git-Bash to open a terminal window.

<img src="https://qloudableassets.blob.core.windows.net/oci-learninglibrary/qloudable/Using_Custom_Image/img/3.png?st=2020-04-17T13%3A29%3A35Z&se=2023-04-18T13%3A29%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=qiutPpXR23TPkCPGckbB84JaQFAwUef0NgOByiqtTXM%3D" alt="image-alt-text">

2. Enter command

`ssh-keygen`

**HINT:** You can swap between OCI window, 
git-bash sessions and any other application (Notepad, etc.) by Clicking the Switch Window icon 

<img src="https://qloudableassets.blob.core.windows.net/oci-learninglibrary/qloudable/Using_Custom_Image/img/4.png?st=2020-04-17T13%3A29%3A35Z&se=2023-04-18T13%3A29%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=qiutPpXR23TPkCPGckbB84JaQFAwUef0NgOByiqtTXM%3D" alt="image-alt-text">

3. Press Enter When asked for 'Enter File in which to save the key', 'Created Directory, 'Enter passphrase', and 'Enter Passphrase again.

<img src="https://qloudableassets.blob.core.windows.net/oci-learninglibrary/qloudable/Using_Custom_Image/img/5.png?st=2020-04-17T13%3A29%3A35Z&se=2023-04-18T13%3A29%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=qiutPpXR23TPkCPGckbB84JaQFAwUef0NgOByiqtTXM%3D" alt="image-alt-text">

4. You should now have the Public and Private keys:

/C/Users/ PhotonUser/.ssh/id_rsa (Private Key)

/C/Users/PhotonUser/.ssh/id_rsa.pub (Public Key)

**NOTE:** id_rsa.pub will be used to create 
Compute instance and id_rsa to connect via SSH into compute instance.

**HINT:** Enter command

`cd /C/Users/PhotonUser/.ssh (No Spaces)`

and then

`ls`

to verify the two files exist.

5. In git-bash Enter command  

`cat /C/Users/PhotonUser/.ssh/id_rsa.pub`

highlight the key and copy

<img src="https://qloudableassets.blob.core.windows.net/oci-learninglibrary/qloudable/Using_Custom_Image/img/6.png?st=2020-04-17T13%3A29%3A35Z&se=2023-04-18T13%3A29%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=qiutPpXR23TPkCPGckbB84JaQFAwUef0NgOByiqtTXM%3D" alt="image-alt-text">

6. Click the apps icon, launch notepad and paste the key in Notepad (as backup)

<img src="https://qloudableassets.blob.core.windows.net/oci-learninglibrary/qloudable/Using_Custom_Image/img/7.png?st=2020-04-17T13%3A29%3A35Z&se=2023-04-18T13%3A29%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=qiutPpXR23TPkCPGckbB84JaQFAwUef0NgOByiqtTXM%3D" alt="image-alt-text">

7. Switch to the OCI console. From OCI services menu, Click **Instances** under **Compute** 

8. Click **Create Instance**. Fill out the dialog box:

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

<img src="https://qloudableassets.blob.core.windows.net/oci-learninglibrary/qloudable/Using_Custom_Image/img/8.png?st=2020-04-17T13%3A29%3A35Z&se=2023-04-18T13%3A29%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=qiutPpXR23TPkCPGckbB84JaQFAwUef0NgOByiqtTXM%3D" alt="image-alt-text">

- **Boot Volume:** Leave the default
- **Add SSH Keys:** Choose 'Paste SSH Keys' and paste the Public Key saved earlier.

9. Click **Create**

**NOTE:** If 'Service limit' error is displayed choose a different shape from VM.Standard2.1, VM.Standard.E2.1, VM.Standard1.1, VM.Standard.B1.1  OR choose a different AD

<img src="https://qloudableassets.blob.core.windows.net/oci-learninglibrary/qloudable/Using_Custom_Image/img/9.png?st=2020-04-17T13%3A29%3A35Z&se=2023-04-18T13%3A29%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=qiutPpXR23TPkCPGckbB84JaQFAwUef0NgOByiqtTXM%3D" alt="image-alt-text">

10. Wait for Instance to be in **Running** state. In git-bash Enter Command:

`cd /C/Users/PhotonUser/.ssh`

11. Enter **ls** and verify id_rsa file exists

12. Enter command 

`ssh -i id_rsa opc@<PUBLIC_IP_OF_COMPUTE>`

**HINT:** If 'Permission denied error' is seen, ensure you are using '-i' in the ssh command. You MUST type the command, do NOT copy and paste ssh command

13. Enter 'Yes' when prompted for security message

<img src="https://qloudableassets.blob.core.windows.net/oci-learninglibrary/qloudable/Using_Custom_Image/img/10.png?st=2020-04-17T13%3A29%3A35Z&se=2023-04-18T13%3A29%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=qiutPpXR23TPkCPGckbB84JaQFAwUef0NgOByiqtTXM%3D" alt="image-alt-text">
 
14. Verify opc@<COMPUTE_INSTANCE_NAME> appears on the prompt

## Install httpd on compute instance and create custom image

1. Switch to ssh session to compute install. Install httpd server, Enter Command:

`sudo yum -y install httpd`

2. Start httpd, Enter command:

`sudo systemctl start httpd`

3. Verify http status, Enter command:

`sudo service httpd status`

4. We now have installed httpd server on a compute instance and will create a custom image. Switch back to OCI Console window

5. From OCI services menu, Click **Instances** under **Compute**.

6. Click your compute instance name and Click **Stop**

<img src="https://qloudableassets.blob.core.windows.net/oci-learninglibrary/qloudable/Using_Custom_Image/img/11.png?st=2020-04-17T13%3A29%3A35Z&se=2023-04-18T13%3A29%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=qiutPpXR23TPkCPGckbB84JaQFAwUef0NgOByiqtTXM%3D" alt="image-alt-text">

7. Once Stopped, Click **Create Custom Image** from **Actions** menu.

<img src="https://qloudableassets.blob.core.windows.net/oci-learninglibrary/qloudable/Using_Custom_Image/img/12.png?st=2020-04-17T13%3A29%3A35Z&se=2023-04-18T13%3A29%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=qiutPpXR23TPkCPGckbB84JaQFAwUef0NgOByiqtTXM%3D" alt="image-alt-text">

8. Fill out the dialog box and Click **Create Custom Image**. VMs status will change to **Creating Image**.

<img src="https://qloudableassets.blob.core.windows.net/oci-learninglibrary/qloudable/Using_Custom_Image/img/13.png?st=2020-04-17T13%3A29%3A35Z&se=2023-04-18T13%3A29%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=qiutPpXR23TPkCPGckbB84JaQFAwUef0NgOByiqtTXM%3D" alt="image-alt-text">

9. Navigate to main Instances page under compute and Click **Custom Images**. Locate your custom image, Click the Action icon and then **Create Instance**.

<img src="https://qloudableassets.blob.core.windows.net/oci-learninglibrary/qloudable/Using_Custom_Image/img/14.png?st=2020-04-17T13%3A29%3A35Z&se=2023-04-18T13%3A29%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=qiutPpXR23TPkCPGckbB84JaQFAwUef0NgOByiqtTXM%3D" alt="image-alt-text">

10. Fill out the dialog box and Click **Create**. Once the instance is in running state note down it's Public IP address.

11. ssh to compute instance as before and Enter command:

`sudo service httpd start`

12. This will start httpd service, Check the status of httpd service as before

**You have successfully created a custom image with httpd already installed and used this custom image to launch a compute instance and started httpd service. In this new compute instance there was no need to re-install httpd server as it was already present when the custom image was created.**

**A compute instance can have a lot more applications installed and this custom image feature facilitates launching new compute instances with these applications pre-installed.**

## Delete the resources

1. Switch to  OCI console window

2. If your Compute instance is not displayed, From OCI services menu Click Instances under Compute

3. Locate first compute instance, Click Action icon and then **Terminate** 

<img src="https://qloudableassets.blob.core.windows.net/oci-learninglibrary/qloudable/Using_Custom_Image/img/15.png?st=2020-04-17T13%3A29%3A35Z&se=2023-04-18T13%3A29%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=qiutPpXR23TPkCPGckbB84JaQFAwUef0NgOByiqtTXM%3D" alt="image-alt-text">

4. Make sure Permanently delete the attached Boot Volume is checked, Click Terminate Instance. Wait for instance to fully Terminate

<img src="https://qloudableassets.blob.core.windows.net/oci-learninglibrary/qloudable/Using_Custom_Image/img/16.png?st=2020-04-17T13%3A29%3A35Z&se=2023-04-18T13%3A29%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=qiutPpXR23TPkCPGckbB84JaQFAwUef0NgOByiqtTXM%3D" alt="image-alt-text">

5. Repeat the step to delete second compute instance

6. From OCI Servies Menu Click **Compute** then **Custom Images**. Locate the custom image you created. Click the Action icon and then **Terminate**

7. From OCI services menu Click **Virtual Cloud Networks** under Networking, list of all VCNs will 
appear.

8. Locate your VCN , Click Action icon and then **Terminate**. Click **Delete All** in the Confirmation window. Click **Close** once VCN is deleted

<img src="https://qloudableassets.blob.core.windows.net/oci-learninglibrary/qloudable/Using_Custom_Image/img/17.png?st=2020-04-17T13%3A29%3A35Z&se=2023-04-18T13%3A29%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=qiutPpXR23TPkCPGckbB84JaQFAwUef0NgOByiqtTXM%3D" alt="image-alt-text">

***Congratulations! You have successfully completed the lab.***
