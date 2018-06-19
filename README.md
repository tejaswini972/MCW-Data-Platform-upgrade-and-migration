# Data platform upgrade and migration

[Download Cloud Workshop](https://github.com/Microsoft/MCW-Data-platform-upgrade-and-migration/archive/master.zip)

World Wide Importers (WWI) has experienced significant growth in the last few years. In addition to predictable growth, they’ve had a substantial amount of growth in the data they store in their data warehouse. Their data warehouse is starting to show its age; slowing down during extract, transform, and load (ETL) operations and during critical queries. It was built on SQL Server 2008 R2 Standard Edition.

The WWI CIO has recently read about new performance enhancements of SQL Server 2017. She is excited about clustered ColumnStore indexes. In addition, she has chosen to upgrade to Enterprise Edition. She’s hoping that table compression will improve performance, backup times, and lessen the load on the Storage Area Network (SAN).

WWI is concerned about upgrading their database to SQL Server 2017 or Azure SQL Database. The data warehouse has been successful for a long time. As it has grown, it has filled with data, stored procedures, views, and security. WWI wants assurance that if it moves its data store, it won’t run into any incompatibilities with the storage engine of SQL Server 2017.

WWI’s CIO would like a POC of a data warehouse move and proof that the new technology will help ETL and query performance.

## Target Audience

* Application developer
* SQL Developer
* Database Administrator

## Abstract

### Workshop

In this workshop, you will gain a better understand of how to conduct a site analysis for a customer to compare cost, performance, and level of effort required to migrate from Oracle to SQL Server. You will evaluate the dependent applications and reports that will need to be updated, and come up with a migration plan. In addition, you will design and build a Proof-of-Concept (POC) and help the customer take advantage of new SQL Server features to improve performance and resiliency, as well as explore ways to migrate from an old version of SQL Server to the latest version and consider the impact of migrating from on-premises to the cloud.

At the end of this workshop, you will be better able to conduct a site analysis for compare cost, performance, and level of effort required to migrate from Oracle to SQL Server.

### Whiteboard design session

In this whiteboard design session, you will design a proof of concept (POC) for conducting a site analysis for a customer to compare cost, performance, and level of effort required to migrate from Oracle to SQL Server. You will evaluate the dependent applications and reports that will need to be updated, and come up with a migration plan. In addition, you will look at ways to help the customer take advantage of new SQL Server features to improve performance and resiliency, as well as explore ways to migrate from an old version of SQL Server to the latest version and consider the impact of migrating from on-premises to the cloud.

At the end of this whiteboard design session, you will be better able to design a database migration plan and implementation.

### Hands-on lab

In this hands-on lab, you will implement a proof of concept (POC) for conducting a site analysis for a customer to compare cost, performance, and level of effort required to migrate from Oracle to SQL Server. You will evaluate the dependent applications and reports that will need to be updated, and come up with a migration plan. In addition, you will help the customer take advantage of new SQL Server features to improve performance and resiliency, as well as conduct a migration from an old version of SQL Server to Azure SQL Database.

At the end of this hands-on lab, you will be better able to design and build a database migration plan and implement any required application changes associated with changing database technologies.

## Azure Services and Related Products

* Azure App Services
* Azure SQL Database
* Azure SQL Data Warehouse
* Data Migration Assistant (DMA)
* SQL Server 2008 and 2017
* SQL Server Management Studio (SSMS)
* SQL Server Migration Assistant (SSMA)
* Visual Studio 2017

## Azure Solution

## Related References

[MCW](https://github.com/Microsoft/MCW)