## Introduction

[Oracle Cloud VMware Solution](Link) (OCVS) is a fully certified, supported and secure VMware software defined data centre (SDDC) that is hosted on Oracle Cloud Infrastructure (OCI) and uses VMware software and licenses.

The base configuration includes three Oracle Cloud Infrastructure Compute bare metal hosts (BM.DenseIO2.52). This configuration can be altered to allow a single Oracle Cloud VMware Solution SDDC to use 64 bare metal hosts. The base configuration also includes an Oracle Cloud Infrastructure virtual cloud network (VCN), 156 OPCUs, 2304 GB of physical memory, and 153 TB of NVMe-based raw storage.

The solution includes VMware software such as vSphere, vSAN, NSX-T, and vCenter Server. The vSAN converged storage technology ensures the availability of data and replicates data across all the bare metal hosts in the SDDC.

These hands-on lab guides provide step-by-step directions to set up and use your VMware Solution SDDC in the Oracle Cloud Infrastructure.

## Goals for this workshop
1. Provision the infrastructure for the SDDC.
2. Export and migrate on-premises workload to the SDDC.
3. Extend application running on OCVS by integrating it with OCI native services.

## Prerequisites
1. 3 bare metal hosts (BM.DenseIO2.52) - Your tenancy should have access to at least 3 BM.DenseIO2.52 shape compute instances in a single Availability Domain.
2. A Virtual Cloud Network (VCN) - You need to either create a virtual cloud network or you can use an existing VCN to deploy the SDDC. We recommend using a VCN with an IP address CIDR size of /22 or greater to run the SDDC. 
3. The user should have the privileges to deploy Bare metal compute instances, and  to create or update VCNs, Load Balancers and Object Storage Buckets. 

## Labs
Lab 100 - Provision the Infrastructure.

Lab 200 - Generate Export of On-Premises workload.

Lab 300 - Migrate the On-Premises workload to OCVS. 

Lab 300 - Extend/Integrate Apps running on OCVS to OCI native services.

**You are all set. Let us begin!**