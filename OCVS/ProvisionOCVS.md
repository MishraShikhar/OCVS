# Provisioning Oracle Cloud VMware Solution

## Introduction

This lab will get you started with the **Oracle Cloud VMware Service**. In this lab, you will provision a Oracle Cloud VMware Stack including the Oracle Cloud Infrastructure resources needed to host the solution.

To log issues and view the Lab Guide source, go to the [github oracle](https://github.com/oracle/learning-library/issues/new) repository.

## Objectives

As a System Administrator or application developer:

- Rapidly deploy OCVS on Oracle cloud Infrastructure 
- Manage your VMware workloads

## Required Artifacts

- An Oracle Cloud Infrastructure account with service limit of atleast **3 Bare Metal DenselIO 2.52** compute shape in the same availability domain.
- A **virtual cloud network** with a recommended CIDR size of **/22** or greater. 
- A subnet within the aforementioned vcn with a **/24** CIDR size for the workloads.


## Steps

### STEP 1: Create an SDDC


-  Sign in to the OCI console andn open the navigation menu by clicking on the hamburger menu icon on the top left of the screen.

-  Under **Solutions and Platform**, click on **VMware Solution**.

    ![](./images/Lab100/100_1.png " ")

- Select the compartment where you wish to deploy the solution from the **Compartment** drop down on the left side of the screen.

- Click on the **Create SDDC** button.

    ![](./images/Lab100/100_2.png " ")

**Note**: Oracle Cloud Infrastructure allows logical isolation of users within a tenancy through Compartments. This allows multiple users and business units to share an OCI tenancy while being isolated from each other.

**If you have chosen a compartment where you do not have the required privileges, then you will not be able to provision the Solution.**

More information about Compartments and Policies is provided in the OCI Identity and Access Management documentation [here](https://docs.cloud.oracle.com/iaas/Content/Identity/Tasks/managingcompartments.htm?tocpath=Services%7CIAM%7C_____13).

-  On the Basic information page, provide the following details:-
    - **SDDC Name**: A descriptive name for the SDDC, such as 'SDDCVMWare'. This name has to be unique among all SDDCs across all compartments in the region. The name must have 1 to 16 characters, must start with a letter, can contain only alphanumeric characters and hyphens (-), and is not case-sensitive.
    - **SDDC Compartment**: The deployment compartment. The destination compartment can be changed from here, as well.
    - **VMware Software Version**: The version of bundled VMware software that you want to install on the ESXi hosts. The software bundle includes vSphere, vSAN, and NSX components. For the purpose of this lab, please select version **6.7**.
    - **Number of ESXi Hosts**: The initial number of ESXi hosts to create in the SDDC. This number has to be at least 3 and can be at most 64. Please choose **3** for this lab.
    
    ![](./images/Lab100/100_4.png " ")

    **Note:** The Oracle Cloud VMware Solution supports addition and deletion of ESXi hosts in the SDDC. 
    
    - **Prefix for ESXi Hosts**: (Optional) The optional prefix that you would like to use for the names of the ESXi hosts for identification. This string follows the same rules, as the SDDC name.
    - **SSH Key**: The public key portion of the SSH key that will be used for remote connections to the ESXi hosts. 
    - **Availability Domain**: The availability domain in which the SDDC and the ESXi hosts will be created. The management subnet and VLANs for this SDDC must be in the same availability domain. 
    
    **Note**: ESXi hosts are uniformly distributed across fault domains within the availability domain. 

- Click Next.

    ![](./images/Lab100/100_5.png " ")

- On the SDDC Networks page, provide the following values:- 
    - **Virtual Cloud Network (VCN)**: The VCN represents the underlying data center network that hosts the SDDC. The VCN can be in a different compartment than the SDDC and its ESXi hosts. 
    - **Create New Subnet and VLAN**: If the network resources for this SDDC have to be created, then this option has to be selected, otherwise enter the details of the subnet that you wish to use.
    - **SDDC Management CIDR**: An available CIDR block in the selected VCN for the SDDC management CIDR. 
    
    The SDDC Management CIDR is divided into eight segments, one for the provisioning subnet and seven for VLANs. As previously mentioned, the size must be at least /22 to allow the maximum of 64 ESXi hosts to each have their own IP address. Clicking on Check Availability will help ensure that the selected CIDR block is available in the VCN. 
    
    ![](./images/Lab100/100_6.png " ")

    - **NSX Network**: The SDDC workload CIDR block. This CIDR block provides the IP addresses in the SDDC to be used by the VMware VMs to run workloads. The value must be /30 or larger and must not overlap with the VCN CIDR block.

        ![](./images/Lab100/100_7.png " ")

    - Click on **Next** to review a summary of the settings for creating the SDDC. If everything is correct, click on **Create SDDC**.

        ![](./images/Lab100/100_8.png " ")

        ![](./images/Lab100/100_9.png " ")

    The summary page tells you that the SDDC creation request has been initiated and shows the provisioning status of each resource. The SDDC should be up and running in, roughly, two and a half hours.

    ![](./images/Lab100/100_10.png " ")

- To monitor the progress of the SDDC creation, click on the URL at the top of the summary page. The SDDC details page will open. 

    ![](./images/Lab100/100_11.png " ")

### STEP 2: Create a Bastion host to access your SDDC

- Once the SDDC is up and running, we will need a public compute instance in the same VCN, as the SDDC, to act as the bastion host.

- Open the navigation menu by clicking on the hamburger menu icon on the top left of the screen.

- Under **Core Infrastructure**, click on **Compute** and then on **Instances**.

    ![](./images/Lab100/100_12.png " ")

- On the instances page, click on the **Create Instance** button.

    ![](./images/Lab100/100_13.png " ")

    ![](./images/Lab100/100_14.png " ")

- On the Create Compute Instance page, provide the following values:- 
    - **Name**: The name of the virtual machine. 
    - **Image**: The image/operating system to be used by the virtual machine. We want to use the Windows Server 2016 Standard Image for this machine. 
    
    Clicking on the Change Image button will open a side panel. Scroll down to find the Windows Server 2016 Standard image.

    ![](./images/Lab100/100_15.png " ")

    - **Availability Domain (AD)** - The AD where the instance will be provisioned. 
    - **Shape**: The shape (OCPU / memory) configuraion of the machine. Select a VM.Standard2.1 shape for your compute instance.

    ![](./images/Lab100/100_16.png " ")

    ![](./images/Lab100/100_17.png " ")

    - **VCN**: Select the same VCN that was used for creating the SDDC. Select a public subnet, so that a public IP is assigned to the Virtual Machine.

    ![](./images/Lab100/100_18.png " ")

- Click on the **Create** button.

**Upon creating this instance, both a user name and an initial password will be generated for you. They will be available on the details screen of the newly launched Instance. You must create a new password upon logging into the instance for the first time.**

- Once the VM is created, click on your instance for details.

    ![](./images/Lab100/100_19.png " ")

- Copy the Public IP address, username and one-time password from the console of the VM. you will need them to establish remote desktop connectivity to the machine. 

    ![](./images/Lab100/100_20.png " ")

- Connect to the instance using a Remote Desktop application of your choosing. The screenshots below show the process from a Mac based client.

    ![](./images/Lab100/100_21.png " ")

    ![](./images/Lab100/100_22.png " ")    

    ![](./images/Lab100/100_23.png " ")    

    ![](./images/Lab100/100_24.png " ")

- When you login the first time, you will be asked to set a password. Reset your password and make a note of it.

    ![](./images/Lab100/100_25.png " ")

- Now, we will goto the page of our SDDC and we will copy the vSphere Client vCenter URL.

    ![](./images/Lab100/100_26.png " ")

- Within your Windows machine that you have connected to using the RDP clinet, install the chrome browser and paste the **vSphere Client** link.

    ![](./images/Lab100/100_27.png " ")

 You will get a warning for unprotected access. Click on **Advanced** and then select the **Proceed** option to continue.

![](./images/Lab100/100_28.png " ")

![](./images/Lab100/100_29.png " ")

- Click on the **Launch vSphere Client** button.

    ![](./images/Lab100/100_30.png " ")

- Here you will need to enter the credential available on the SDDC's OCI console page. Go to the SDDC's console page and copy the **vCenter Username** and **vCenter Initial Password**.

![](./images/Lab100/100_31.png " ")

**Note**: You will also get the NSX login information here.

- You should now be able to access the vCenter. From here you can manage the VMware environment.

    ![](./images/Lab100/100_33.png " ")

- If you look at the panel on the left, you should be able to see the backend hosts that we used to provision our environment.

    ![](./images/Lab100/100_34.png " ")
