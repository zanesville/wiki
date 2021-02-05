---
title: Understanding ArcGIS Enterprise
New field 1: Understanding ArcGIS Enterprise
category: documents
---

## Levels of ArcGIS Enterprise

There also exist two levels of ArcGIS Enterprise--ArcGIS Enterprise and ArcGIS Enterprise Workgroup.

### ArcGIS Enterprise level

The ArcGIS Enterprise level is designed for medium to large-sized teams. At this level, enterprise geodatabases are utilized with ArcGIS Enterprise allowing an unlimited number of simultaneous connections to the database. This level comes with one four-core processor license and is scalable with additional two-core add-on packs.

### ArcGIS Enterprise Workgroup level

The ArcGIS Enterprise Workgroup level is designed for smaller teams and organizations, allowing a maximum of 10 simultaneous connections to workgroup and file geodatabases; **enterprise geodatabases are not supported**. The base ArcGIS Enterprise deployment (Server, Portal, Web Adaptor, or Data Store) must be deployed all in one on a single machine with up to four cores. Server roles have a maximum of four cores--no add-on two-core packs are available.

## Basic edition

ArcGIS GIS Server Basic edition includes geodatabase management and the ability to publish read-only feature services. Also included are the geodata service and geometry service. Web editing is not available and this edition cannot be federated with Portal for ArcGIS. No ArcGIS Server extensions are available for purchase and implementation at the Basic edition.

This manages your geodatabase and public feature services (without the ability to edit); it cannot be deployed with Portal for ArcGIS.

[https://assets.esri.com/content/dam/esrisites/en-us/media/brochures/arcgis-enterprise-functionality-matrix.pdf](https://assets.esri.com/content/dam/esrisites/en-us/media/brochures/arcgis-enterprise-functionality-matrix.pdf)

[https://subscription.packtpub.com/book/application_development/9781788297493/1/ch01lvl1sec8/introduction-to-arcgis-enterprise-10-5-1](https://subscription.packtpub.com/book/application_development/9781788297493/1/ch01lvl1sec8/introduction-to-arcgis-enterprise-10-5-1)

***Side note: SQL Server Express does not need an Enterprise Server license, allowing multi-user viewing of data on a networked drive***

### Hardware Requirements

- One database VM, one VM for the base ArcGIS Enterprise Deployment, which includes the following:
 - ArcGIS Server
 - Portal for ArcGIS (not really sure we need this)
 - ArcGIS Data Store (can be hosted on another machine with no extra license)
 - ArcGIS Web Adaptor

The link below will give you all you need on the system requirements.

[https://enterprise.arcgis.com/en/system-requirements/latest/linux/arcgis-enterprise-builder-system-req.htm](https://enterprise.arcgis.com/en/system-requirements/latest/linux/arcgis-enterprise-builder-system-req.htm)

**Summary**
 - 64-bit
 - The fastest hard drives and ram that fits our budget.
  - Total space needed for for **file** storage would be 256 GB.
  - Total space for the PostgreSQL Database Server (on this VM, another VM, or another internal machine) - min 25GB, up to 100GB.
 - no "_" underscore in hostname
 - ArcGIS Server is not supported on domain controllers. Installing ArcGIS Server on a domain controller may adversely affect functionality.
 - Windows Server 2008 R2, 2012, 2016, 2019 all with latest updates
 - Red Hat Enterprise Server 7, 6, Ubuntu Server LTS 18, LTS 16, CentOS 7, 6, others... (ext4 or xfs file system)
 - 8GB Ram per role (we will only be using one role at first) see below
 - The minimum RAM requirement for ArcGIS GIS Server, ArcGIS GeoEvent Server, ArcGIS Image Server, or ArcGIS Business Analyst for Server is 8 GB per unique license role. **We will only be running ArcGIS GIS Server.**
 - The following virtualization environments are known to perform well with ArcGIS Enterprise and its components:
  - VMware vSphere 6.5, 6.7
  - Microsoft Hyper-V
 - Minimum 4 Cores, and beyond that we would need another license, so we will be sticking with a 4 core VM or machine
 - Workgroup is limited to 2 cores, so a 4-core 2VM machine could work, with one VM for ArcGIS Server and one VM for SQL Express, more cores for Server Workgroup (up to 4) are available for more $$
 - An 8-core machine split into 2 VMs would allow us to either expand in the future
 
Very detailed document on IT and ArcGIS architecture - [https://www.esri.com/content/dam/esrisites/en-us/media/pdf/architecting-the-arcgis-platform.pdf](https://www.esri.com/content/dam/esrisites/en-us/media/pdf/architecting-the-arcgis-platform.pdf).

**Below is information about the software licensing.**

---


### Levels of ArcGIS Enterprise

There exists two levels of ArcGIS Enterprise--ArcGIS Enterprise and ArcGIS Enterprise Workgroup.

#### ArcGIS Enterprise level

The ArcGIS Enterprise level is designed for medium to large-sized teams. At this level, enterprise geodatabases are utilized with ArcGIS Enterprise allowing an unlimited number of simultaneous connections to the database. This level comes with one four-core processor license and is scalable with additional two-core add-on packs. Esri Apps can be deployed to the data store managed by this level of Enterprise.

#### ArcGIS Enterprise Workgroup level

The ArcGIS Enterprise Workgroup level is designed for smaller teams and organizations, allowing a maximum of 10 simultaneous connections to workgroup and file geodatabases; **enterprise geodatabases are not supported**. The base ArcGIS Enterprise deployment (Server, Portal, Web Adaptor, or Data Store) must be deployed all in one on a single machine with up to four cores. Server roles have a maximum of four cores--no add-on two-core packs are available.

**Enterprise Workgroup does not allow for the deployment of Esri Apps to the Workgroup data store, meaning if we deploy any of these they would need to be hosted on AGOL. A workaround exists to spin up a PostgreSQL instance and use this as the data store.**

### Basic edition

ArcGIS GIS Server Basic edition includes geodatabase management and the ability to publish read-only feature services. Also included are the geodata service and geometry service. Web editing is not available and this edition cannot be federated with Portal for ArcGIS. No ArcGIS Server extensions are available for purchase and implementation at the Basic edition.

This manages your geodatabase and public feature services (without the ability to edit); it cannot be deployed with Portal for ArcGIS.

[https://assets.esri.com/content/dam/esrisites/en-us/media/brochures/arcgis-enterprise-functionality-matrix.pdf](https://assets.esri.com/content/dam/esrisites/en-us/media/brochures/arcgis-enterprise-functionality-matrix.pdf)

[https://subscription.packtpub.com/book/application_development/9781788297493/1/ch01lvl1sec8/introduction-to-arcgis-enterprise-10-5-1](https://subscription.packtpub.com/book/application_development/9781788297493/1/ch01lvl1sec8/introduction-to-arcgis-enterprise-10-5-1)

### Notes from various online resources

> Geodatabases in AWS instances are not intended to be accessed directly from on-premises ArcGIS clients as performance will be far slower than when the geodatabases are accessed from ArcGIS clients on AWS. - https://enterprise.arcgis.com/en/server/10.6/cloud/amazon/geodatabases-and-arcgis-server-on-aws.htm

This means either running virtual desktops in the cloud at the same location as the Enterprise instance, or running the Enterprise server and the database server locally.

http://proceedings.esri.com/library/userconf/proc17/tech-workshops/tw_240-421.pdf
