## Introduction

In this lab, we will import the on-premises VMWare environment that we had exported in Lab 200 to OCVS. 

## Objectives
- Import an OVF file and access the environment as part of the Oracle Cloud VMWare Service.

## Required Artifacts
- Access to the Object storage bucket used to store the exported workload from Lab 200.
- The deployed OCVS environment from Lab 100.

## Steps

### STEP 1: Import the osCommerce OVF into Oracle Cloud VMware Service

- Use the pre-authenticated request URL from Lab 200 to download the zipped ovf file.  Unzip to extract the 3 ovf files on your computer.

    ![](./images/Lab300/300_1.png " ")

- Use the RDP client to connect to the bastion host. Now, login to the vSphere client of your OCVS platform and enter the credentials. You can get the vCenter server details from your OCVS page.

    ![](./images/Lab300/300_2.png " ")

- Go to **Menu** and select **Hosts and Clusters**.

    ![](./images/Lab300/300_3.png " ")

- Right click on the Host in the VMware Host client Inventory and Select **Deploy OVF Template**.

    ![](./images/Lab300/300_4.png " ")

- Select the .ovf and VMDK file from the unzipped folder.

    ![](./images/Lab300/300_5.png " ")

- Enter a name and select a location for your VM and select a compute resource. Review the details.

    ![](./images/Lab300/300_6.png " ")
    ![](./images/Lab300/300_7.png " ")
    ![](./images/Lab300/300_8.png " ")

- Now, select the storage. After doing so, choose the Workload network on the next screen.

    ![](./images/Lab300/300_9.png " ")
    ![](./images/Lab300/300_10.png " ")

- Click on **Finish** to complete the deployment.

    ![](./images/Lab300/300_11.png " ")

- Select the VM that you just imported and power it on.

    ![](./images/Lab300/300_12.png " ")

- Launch the web console and login to the VM. Open Firefox and enter **localhost/catalog/index.php** and check for the oscommerce index page.

     ![](./images/Lab300/300_13.png " ")

