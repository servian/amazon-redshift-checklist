# Amazon Redshift Checklist

The Amazon Redshift Checklist is an exhaustive list of all elements you need to have / to test before launching your Redshift cluster into production.

[How to use](#how-to-use) • [Contributing](#contributing)

## Table of Contents

1. [Designing Tables](#designing-tables)
2. [Loading Data](#loading-data)
3. [Performance](#performance)
4. [Security](#security)
5. [Monitoring](#monitoring)
6. [Cluster](#cluster)

---

## How to use

All items in the **Amazon Redshift Checklist** are required for the majority of the projects, but some elements can be omitted or are not essential. We choose to use 3 levels of flexibility:

- :o: means that the item is **recommended** but can be omitted in some particular situations.
- :interrobang: means that the item is **highly recommended** and can eventually be omitted in some really particular cases.
- :bangbang: means that the item **can't be omitted** by any reason.

Some resources possess an emoticon to help you understand which type of content / help you may find on the checklist:

- :book: documentation or article
- :wrench: online tool / testing tool
- :video_camera: media or video content

---

## Designing Tables

- [ ] :bangbang: **Table distribution styles:** In order to utilise the parallel nature of Redshift, data must be correctly distributed within each table of the cluster.

  - :book: [Choosing a data distribution style](https://docs.aws.amazon.com/redshift/latest/dg/t_Distributing_data.html)

- [ ] :interrobang: **Column compression:** Ensures data is better compressed utilising less storage space.

  - :book: [Choosing a column compression type](https://docs.aws.amazon.com/redshift/latest/dg/t_Compressing_data_on_disk.html)

- [ ] :interrobang: **Table sort keys:** Ensures data is retrieved from within each node in the most performant way.

  - :book: [Choosing sort keys](https://docs.aws.amazon.com/redshift/latest/dg/t_Sorting_data.html)

- [ ] :o: **Table constraints:** Not enforced by Redshift but helps query optimisers.

  - :book: [Defining constraints](https://docs.aws.amazon.com/redshift/latest/dg/t_Defining_constraints.html)

[:arrow_up: back to top](#table-of-contents)

---

## Loading Data

- [ ] :bangbang: **COPY command:** Load files from S3 to Redshift via COPY to ensure parallel quick loads.

  - :book: [Use a COPY command to load data](https://docs.aws.amazon.com/redshift/latest/dg/c_best-practices-use-copy.html)

- [ ] :interrobang: **Compress data files:** Use either GZIP, LZOP, BZIP2, or ZSTD.

  - :book: [Compress your data files](https://docs.aws.amazon.com/redshift/latest/dg/c_best-practices-compress-data-files.html)

- [ ] :interrobang: **Multi-row insert:** If a COPY command is not an option and you require SQL inserts, use a multi-row insert whenever possible.

  - :book: [Use a multi-row insert](https://docs.aws.amazon.com/redshift/latest/dg/c_best-practices-multi-row-inserts.html)

- [ ] :interrobang: **Sort key order:** Load your data in sort key order to avoid needing to vacuum.

  - :book: [Load data in sort key order](https://docs.aws.amazon.com/redshift/latest/dg/c_best-practices-sort-key-order.html)

- [ ] :interrobang: **Automatic compression:** Use the COPY command with `COMPUPDATE` set to ON to automatically set column encoding for brand new tables

  - :book: [Loading tables with automatic compression](https://docs.aws.amazon.com/redshift/latest/dg/c_Loading_tables_auto_compress.html)

- [ ] :o: **Split data into multiple files:** Split your load data files so that the files are about equal size, between 1 MB and 1 GB after compression.

  - :book: [Split your load data into multiple files](https://docs.aws.amazon.com/redshift/latest/dg/c_best-practices-use-multiple-files.html)

[:arrow_up: back to top](#table-of-contents)

---

## Performance

- [ ] :bangbang: **Automatic workload management (WLM):** Amazon Redshift determines how many concurrent queries and how much memory is allocated to each dispatched query.

  - :book: [Implementing workload management](https://docs.aws.amazon.com/redshift/latest/dg/cm-c-implementing-workload-management.html)

- [ ] :interrobang: **Concurrency scaling:** Dynamically adds concurrent clusters improving read query concurrency.

  - :book: [Working with concurrency scaling](https://docs.aws.amazon.com/redshift/latest/dg/concurrency-scaling.html)

- [ ] :interrobang: **:new: AZ64 column compression encoding:** Consider using Redshift's proprietery new column encoding algorithm AZ64.

  - :book: [Amazon Redshift introduces AZ64, a new compression encoding for optimized storage and high query performance](https://aws.amazon.com/about-aws/whats-new/2019/10/amazon-redshift-introduces-az64-a-new-compression-encoding-for-optimized-storage-and-high-query-performance/)

- [ ] :o: **Automatic compression:** Use the COPY command with COMPUPDATE set to ON. Note this will impact performance and should only be used on the first load.

  - :book: [ANALYZE COMPRESSION](https://docs.aws.amazon.com/redshift/latest/dg/r_ANALYZE_COMPRESSION.html)

- [ ] :o: **Analyse query performance:** `STL_ALERT_EVENT_LOG` table allows users to analyse and improve performance issues.

  - :book: [STL_ALERT_EVENT_LOG](https://docs.amazonaws.cn/en_us/redshift/latest/dg/r_STL_ALERT_EVENT_LOG.html)

- [ ] :o: **:new: Materialized views:** Consider using materialized views for frequently queried statements or views.

  - :book: [Amazon Redshift materialized views support external tables](https://aws.amazon.com/about-aws/whats-new/2020/06/amazon-redshift-materialized-views-support-external-tables/)

[:arrow_up: back to top](#table-of-contents)

---

## Security

- [ ] :bangbang: **Cluster encryption:** Ensure cluster encryption is turned on protecting data at rest.

  - :book: [Amazon Redshift database encryption](https://docs.amazonaws.cn/en_us/redshift/latest/mgmt/working-with-db-encryption.html)

- [ ] :bangbang: **Publicly accessible:** Most clusters should not be publicly accessible and therefore should be set to private.

  - :book: [How can I make a private Amazon Redshift cluster publicly accessible?](https://aws.amazon.com/premiumsupport/knowledge-center/redshift-cluster-private-public/)

- [ ] :bangbang: **Enhanced VPC routing:** Forces all COPY and UNLOAD traffic between your cluster and your data repositories through your Amazon VPC.

  - :book: [Amazon Redshift enhanced VPC routing](https://docs.aws.amazon.com/redshift/latest/mgmt/enhanced-vpc-routing.html)

- [ ] :bangbang: **User groups:** Create different user groups and grant privileges based on their roles.

  - :book: [Example for controlling user and group access](https://docs.aws.amazon.com/redshift/latest/dg/t_user_group_examples.html)

- [ ] :interrobang: **:new: Federated user access:** Consider providing user access via SAML-2.0 using AD FS, PingFederate, Okta, or Azure AD.

  - :book: [Federate Database User Authentication Easily with IAM and Amazon Redshift](https://aws.amazon.com/blogs/big-data/federate-database-user-authentication-easily-with-iam-and-amazon-redshift/)
  - :book: [Federate Amazon Redshift access with Microsoft Azure AD single sign-on](https://aws.amazon.com/blogs/big-data/federate-amazon-redshift-access-with-microsoft-azure-ad-single-sign-on/)
  - :book: [Federate Amazon Redshift access with Okta as an identity provider](https://aws.amazon.com/blogs/big-data/federate-amazon-redshift-access-with-okta-as-an-identity-provider/)
  - :book: [Options for providing IAM credentials](https://docs.aws.amazon.com/redshift/latest/mgmt/options-for-providing-iam-credentials.html)

- [ ] :interrobang: **:new: Multi-factor authentication (MFA):** Consider enabling MFA for production workloads.

  - :book: [Amazon Redshift introduces support for multi-factor authentication](https://aws.amazon.com/about-aws/whats-new/2020/04/amazon-redshift-introduces-support-multi-factor-authentication/)

[:arrow_up: back to top](#table-of-contents)

---

## Monitoring

- [ ] :bangbang: **Redshift Advisor:** Redshift advisor analyses your cluster and makes recommendation to improve performance and decrease costs.

  - :book: [Viewing Amazon Redshift Advisor recommendations on the console](https://docs.aws.amazon.com/redshift/latest/dg/access-advisor.html)

- [ ] :bangbang: **Long running queries:** Set an alarm to notify users when queries are running for longer than expected using the QueryDuration CloudWatch metric.

  - :book: [Amazon Redshift performance data](https://docs.aws.amazon.com/redshift/latest/mgmt/metrics-listing.html)

- [ ] :bangbang: **Underutilised or Over Utilised Cluster:** Check if your cluster is under utilised or over utilised using the CPUUtilisation CloudWatch metric.

  - :book: [Amazon Redshift performance data](https://docs.aws.amazon.com/redshift/latest/mgmt/metrics-listing.html)

- [ ] :bangbang: **Disk space usage:** Check if your cluster is running out of disk space and whether you need to consider scaling using the PercentageDiskSpaceUsed metric.

  - :book: [Amazon Redshift performance data](https://docs.aws.amazon.com/redshift/latest/mgmt/metrics-listing.html)

- [ ] :bangbang: **:new: CloudWatch anomaly detection:** Applies machine-learning algorithms to the metric's past data to create a model of the metric's expected values.

  - :book: [Using CloudWatch Anomaly Detection](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Anomaly_Detection.html)

- [ ] :interrobang: **Workload performance:** Optimise your cluster based on how much time queries spend on different stages of processing.

  - :book: [Analyzing workload performance](https://docs.aws.amazon.com/redshift/latest/mgmt/analyze-workload-performance.html)

- [ ] :interrobang: **Advanced Redshift monitoring:** Use Lambda and CloudWatch events to generate alarms for common possible issues.

  - :wrench: [Redshift Advance Monitoring](https://github.com/awslabs/amazon-redshift-monitoring)

[:arrow_up: back to top](#table-of-contents)

---

## Cluster

- [ ] :bangbang: **:new: RA3 nodes** Consider using Redshift's new RA3 nodes with a mix of local cache and S3 backed elastic storage if compute requirements exceed dense compute or dense storage node levels.

  - :book: [Amazon Redshift introduces RA3 nodes with managed storage enabling independent compute and storage scaling](https://aws.amazon.com/about-aws/whats-new/2019/12/amazon-redshift-announces-ra3-nodes-managed-storage/)

- [ ] :bangbang: **Automated snapshot retention:** The default retention period of 1 day can catch organisations out in case of disaster recovery or rollback. Conside changing to 35 days.

  - :book: [ModifySnapshotCopyRetentionPeriod](https://docs.aws.amazon.com/redshift/latest/APIReference/API_ModifySnapshotCopyRetentionPeriod.html)

- [ ] :o: **Redshift Spectrum:** Consider Redshift Spectrum allows users to query data straight from S3 using a Redshift cluster. This can be used in replacement of a staging schema whereby your stage data lives within your data lake and is ready into Redshift via Spectrum

  - :book: [Getting started with Amazon Redshift Spectrum](https://docs.aws.amazon.com/redshift/latest/dg/c-getting-started-using-spectrum.html)
  - :book: [Why you’re better off exporting your data to Redshift Spectrum, instead of Redshift](https://mixpanel.com/blog/why-youre-better-off-exporting-your-data-to-redshift-spectrum-instead-of-redshift/)

- [ ] :o: **:new: Pause and resume:** Redshift has recently introduced the ability to pause and resume the cluster within minutes.

  - :book: [Pausing and resuming clusters](https://docs.aws.amazon.com/redshift/latest/mgmt/managing-cluster-operations.html#rs-mgmt-pause-resume-cluster)
  - :wrench: [Redshift Smart Pause and Resume](https://github.com/servian/aws-redshift-smart-pause-and-resume)

- [ ] :o: **:new: Elastic resize:** Consider using elastic resize over classic resize when changing both the node types and number of nodes within your Redshift cluster. Elastic resize is much quicker (minutes vs hours) and doesn't take your cluster out of commision.

  - :book: [Amazon Redshift now supports changing node types within minutes with elastic resize](https://aws.amazon.com/about-aws/whats-new/2020/04/amazon-redshift-now-supports-changing-node-types-within-minutes-with-elastic-resize/)

[:arrow_up: back to top](#table-of-contents)

---

## Contributing

Open an issue or a pull request to suggest changes or additions.

[:arrow_up: back to top](#table-of-contents)
