# Amazon Redshift Checklist

This checklist aims to be an exhaustive list of all elements you should consider when using Amazon Redshift.

## Table of Contents

- [How to use](#how-to-use)
- [Sister Projects](#sister-projects)
- [Checklist](#checklist)
  - [Designing Tables](#designing-tables)
  - [Loading Data](#loading-data)
  - [Performance](#performance)
  - [Security](#security)
  - [Monitoring](#monitoring)
  - [Consumption](#consumption)
  - [Cluster](#cluster)
- [Contributing](#contributing)

## How to use

All items in the **Amazon Redshift Checklist** are required for the majority of projects, but some elements can be omitted or are not essential. We choose to use 3 levels of flexibility:

- :red_circle: means the item **can't be omitted** for any reason.
- :yellow_circle: means the item is **highly recommended** and can eventually be omitted in some really particular cases.
- :green_circle: means the item is **recommended** but can be omitted in some particular situations.

Some resources possess an emoticon to help you understand which type of content/help you may find on the checklist:

- :book: documentation or article
- :wrench: online tool
- :video_camera: media

## Sister Projects

- [Amazon S3 Checklist](https://github.com/servian/amazon-s3-checklist)

## Checklist

### Designing Tables

#### :red_circle: Select an appropriate table distribution style

In order to utilise the parallel nature of Redshift, data must be correctly distributed within each table of the cluster. Tables not distributed correctly (based on their query patterns) will generally lead to poor query performance.

- :book: [Choosing a data distribution style](https://docs.aws.amazon.com/redshift/latest/dg/t_Distributing_data.html)
- :book: [Amazon Redshift now recommends distribution keys for improved query performance](https://aws.amazon.com/about-aws/whats-new/2019/08/amazon-redshift-now-recommends-distribution-keys-for-improved-query-performance/)

#### :yellow_circle: Set column compression

Ensures data is better compressed utilising less storage space.

- :book: [Choosing a column compression type](https://docs.aws.amazon.com/redshift/latest/dg/t_Compressing_data_on_disk.html)
- :book: [Use AZ64 column compression encoding](#yellow_circle-new-use-az64-column-compression-encoding)

#### :yellow_circle: Select appropriate table sort keys

Ensures data is retrieved from within each node in the most performant way.

- :book: [Choosing sort keys](https://docs.aws.amazon.com/redshift/latest/dg/t_Sorting_data.html)
- :book: [Amazon Redshift now supports changing table sort keys dynamically](https://aws.amazon.com/about-aws/whats-new/2019/11/amazon-redshift-supports-changing-table-sort-keys-dynamically/)
- :book: [Amazon Redshift now recommends sort keys for improved query performance](https://aws.amazon.com/about-aws/whats-new/2020/03/amazon-redshift-now-recommends-sort-keys-for-improved-query-performance/)
- :book: [Compound and Interleaved Sort Keys](https://aws.amazon.com/blogs/big-data/amazon-redshift-engineerings-advanced-table-design-playbook-compound-and-interleaved-sort-keys/)

#### :green_circle: Define table constraints

Uniqueness, primary key, and foreign key constraints are informational only; they are not enforced by Amazon Redshift. Nonetheless, primary keys and foreign keys are used as planning hints and they should be declared if your ETL process or some other process in your application enforces their integrity.

- :book: [Defining constraints](https://docs.aws.amazon.com/redshift/latest/dg/t_Defining_constraints.html)

### Loading Data

#### :red_circle: Use the COPY command

Loads data into a table from data files or from an Amazon DynamoDB table. The files can be located in an Amazon Simple Storage Service (Amazon S3) bucket, an Amazon EMR cluster, or a remote host that is accessed using a Secure Shell (SSH) connection.

- :book: [Use a COPY command to load data](https://docs.aws.amazon.com/redshift/latest/dg/c_best-practices-use-copy.html)

#### :yellow_circle: Compress data files

Compressed files generally load faster. Use either GZIP, LZOP, BZIP2, or ZSTD.

- :book: [Compress your data files](https://docs.aws.amazon.com/redshift/latest/dg/c_best-practices-compress-data-files.html)
- :book: [Redshift database benchmarks: COPY performance with compressed files
  ](https://www.stitchdata.com/blog/redshift-database-benchmarks-copy-performance-with-compressed-files/)

#### :yellow_circle: Use multi-row inserts

If a COPY command is not an option and you require SQL inserts, use a multi-row insert whenever possible.

- :book: [Use a multi-row insert](https://docs.aws.amazon.com/redshift/latest/dg/c_best-practices-multi-row-inserts.html)

#### :yellow_circle: Pre-sort data files in a sort key order

Load your data in sort key order to avoid needing to vacuum.

- :book: [Load data in sort key order](https://docs.aws.amazon.com/redshift/latest/dg/c_best-practices-sort-key-order.html)

#### :yellow_circle: Enable automatic compression

Use the COPY command with `COMPUPDATE` set to `ON` to automatically set column encoding for new tables during their first load.

- :book: [Loading tables with automatic compression](https://docs.aws.amazon.com/redshift/latest/dg/c_Loading_tables_auto_compress.html)

#### :green_circle: Split data into multiple files

Split your load data files so that the files are about equal size, between 1 MB and 1 GB after compression.

- :book: [Split your load data into multiple files](https://docs.aws.amazon.com/redshift/latest/dg/c_best-practices-use-multiple-files.html)

### Performance

#### :red_circle: Enable automatic workload management (WLM)

Amazon Redshift determines how many concurrent queries and how much memory is allocated to each dispatched query.

- :book: [Implementing workload management](https://docs.aws.amazon.com/redshift/latest/dg/cm-c-implementing-workload-management.html)
- :video_camera: [Managing Concurrent Workloads with Amazon Redshift](https://www.youtube.com/watch?v=TsHyT-BrSOI)

#### :yellow_circle: Enable concurrency scaling

Dynamically adds concurrent clusters improving read query concurrency.

- :book: [Working with concurrency scaling](https://docs.aws.amazon.com/redshift/latest/dg/concurrency-scaling.html)
- :book: [Concurrency Scaling pricing](https://aws.amazon.com/redshift/pricing/#Concurrency_Scaling_pricing)
- :video_camera: [Amazon Redshift Concurrency Scaling](https://youtu.be/-2yQsI9xJKQ)

#### :yellow_circle: :new: Use AZ64 column compression encoding

Consider using Redshift's proprietary new column encoding algorithm AZ64.

- :book: [Amazon Redshift introduces AZ64, a new compression encoding for optimized storage and high query performance](https://aws.amazon.com/about-aws/whats-new/2019/10/amazon-redshift-introduces-az64-a-new-compression-encoding-for-optimized-storage-and-high-query-performance/)

#### :yellow_circle: Analyse query performance

`STL_ALERT_EVENT_LOG` table allows users to analyse and improve performance issues.

- :book: [STL_ALERT_EVENT_LOG](https://docs.aws.amazon.com/redshift/latest/dg/r_STL_ALERT_EVENT_LOG.html)
- :book: [Evaluating the query plan](https://docs.aws.amazon.com/redshift/latest/dg/c_data_redistribution.html)
- :video_camera: [Query Monitoring with Amazon Redshift](https://www.youtube.com/watch?v=Wdvb5iYVnLg)

#### :green_circle: Disable automatic compression

Use the COPY command with `COMPUPDATE` set to `OFF`. Running compression computing every time on an already known data set will decrease performance.

- :book: [ANALYZE COMPRESSION](https://docs.aws.amazon.com/redshift/latest/dg/r_ANALYZE_COMPRESSION.html)

#### :green_circle: :new: Use materialized views

Materialized views can significantly boost query performance for repeated and predictable analytical workloads such as dashboarding, queries from business intelligence (BI) tools, and ELT (Extract, Load, Transform) data processing.

- :book: [Amazon Redshift materialized views support external tables](https://aws.amazon.com/about-aws/whats-new/2020/06/amazon-redshift-materialized-views-support-external-tables/)

#### :green_circle: Enable short query acceleration (SQA)

SQA runs short-running queries in a dedicated space so that SQA queries aren't forced to wait in queues behind longer queries.

- :book: [Working with short query acceleration](https://docs.aws.amazon.com/redshift/latest/dg/wlm-short-query-acceleration.html)

#### :green_circle: Use elastic resize scheduling

Consider scheduling an elastic cluster resize for nightly ETL workloads or to accommodate heavier workloads during the day as well as shrinking a cluster to accommodate lighter workloads at specific times of the day.

- :book: [Amazon Redshift now supports elastic resize scheduling](https://aws.amazon.com/about-aws/whats-new/2019/11/amazon-redshift-supports-elastic-resize-scheduling/)

#### :green_circle: Use `TRUNCATE` over `DELETE`

Consider using `TRUNCATE` instead of `DELETE` when creating transient tables. `TRUNCATE` is much more efficient than `DELETE` and doesn't require a VACUUM and ANALYZE.

- :book: [TRUNCATE](https://docs.aws.amazon.com/redshift/latest/dg/r_TRUNCATE.html)

### Security

#### :red_circle: Enable cluster encryption

Ensure cluster encryption is turned on protecting data at rest.

- :book: [Amazon Redshift database encryption](https://docs.aws.amazon.com/redshift/latest/mgmt/working-with-db-encryption.html)

#### :red_circle: Disable publicly accessibility

Most clusters should not be publicly accessible and therefore should be set to private.

- :book: [How can I make a private Amazon Redshift cluster publicly accessible?](https://aws.amazon.com/premiumsupport/knowledge-center/redshift-cluster-private-public/)

#### :red_circle: Enable enhanced VPC routing

Forces all COPY and UNLOAD traffic between your cluster and your data repositories through your Amazon VPC.

- :book: [Amazon Redshift enhanced VPC routing](https://docs.aws.amazon.com/redshift/latest/mgmt/enhanced-vpc-routing.html)

#### :red_circle: Use user groups

To make permission management easier, create different user groups and grant privileges based on their roles. Add and remove users to/from groups instead of granting permissions to individual users.

- :book: [Example for controlling user and group access](https://docs.aws.amazon.com/redshift/latest/dg/t_user_group_examples.html)

#### :yellow_circle: :new: Use federated user access

Consider providing user access via SAML-2.0 using AD FS, PingFederate, Okta, or Azure AD.

- :book: [Federate Database User Authentication Easily with IAM and Amazon Redshift](https://aws.amazon.com/blogs/big-data/federate-database-user-authentication-easily-with-iam-and-amazon-redshift/)
- :book: [Federate Amazon Redshift access with Microsoft Azure AD single sign-on](https://aws.amazon.com/blogs/big-data/federate-amazon-redshift-access-with-microsoft-azure-ad-single-sign-on/)
- :book: [Federate Amazon Redshift access with Okta as an identity provider](https://aws.amazon.com/blogs/big-data/federate-amazon-redshift-access-with-okta-as-an-identity-provider/)
- :book: [Options for providing IAM credentials](https://docs.aws.amazon.com/redshift/latest/mgmt/options-for-providing-iam-credentials.html)

#### :yellow_circle: :new: Enable multi-factor authentication (MFA)

Consider enabling MFA for production workloads.

- :book: [Amazon Redshift introduces support for multi-factor authentication](https://aws.amazon.com/about-aws/whats-new/2020/04/amazon-redshift-introduces-support-multi-factor-authentication/)

#### :yellow_circle: Use Secrets Manager for service accounts

Configure AWS Secrets Manager to automatically rotate Amazon Redshift passwords for service accounts. Secrets Manager uses a Lambda function provided by Secrets Manager.

- :book: [Rotating Secrets for Amazon Redshift](https://docs.aws.amazon.com/secretsmanager/latest/userguide/rotating-secrets-redshift.html)
- :book: [Enabling Rotation for an Amazon Redshift Secret](https://docs.aws.amazon.com/secretsmanager/latest/userguide/enable-rotation-redshift.html)

#### :green_circle: :new: Use column-level access controls

Consider implementing column-level access controls to restrict users from accessing certain columns.

- :book: [Announcing column-level access control for Amazon Redshift](https://aws.amazon.com/about-aws/whats-new/2020/03/announcing-column-level-access-control-for-amazon-redshift/)

### Monitoring

#### :red_circle: Action Redshift advisor recommendations

Redshift advisor analyses your cluster and makes recommendations to improve performance and decrease costs.

- :book: [Viewing Amazon Redshift Advisor recommendations on the console](https://docs.aws.amazon.com/redshift/latest/dg/access-advisor.html)

#### :red_circle: Monitor long running queries

Set an alarm to notify users when queries are running for longer than expected using the `QueryDuration` CloudWatch metric.

- :book: [Amazon Redshift performance data](https://docs.aws.amazon.com/redshift/latest/mgmt/metrics-listing.html)

#### :red_circle: Monitor underutilised or over utilised clusters

Check if your cluster is underutilised or over utilised using the `CPUUtilisation` CloudWatch metric.

- :book: [Amazon Redshift performance data](https://docs.aws.amazon.com/redshift/latest/mgmt/metrics-listing.html)
- :wrench: [isitfit](https://github.com/autofitcloud/isitfit)

#### :red_circle: Monitor disk space usage

Check if your cluster is running out of disk space and whether you need to consider scaling using the `PercentageDiskSpaceUsed` metric.

- :book: [Amazon Redshift performance data](https://docs.aws.amazon.com/redshift/latest/mgmt/metrics-listing.html)

#### :red_circle: :new: Enable CloudWatch anomaly detection

Applies machine-learning algorithms to the metric's past data to create a model of the metric's expected values.

- :book: [Using CloudWatch Anomaly Detection](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Anomaly_Detection.html)

#### :red_circle: Query monitoring rules

Define metrics-based performance boundaries for WLM queues and specify what action to take when a query goes beyond those boundaries.

- :book: [WLM query monitoring rules](https://docs.aws.amazon.com/redshift/latest/dg/cm-c-wlm-query-monitoring-rules.html)
- :wrench: [WLM QMR Rule Candidates](https://github.com/awslabs/amazon-redshift-utils/blob/master/src/AdminScripts/wlm_qmr_rule_candidates.sql)
- :wrench: [QMRNotificationUtility](https://github.com/awslabs/amazon-redshift-utils/tree/master/src/QMRNotificationUtility)

#### :yellow_circle: Analyse workload performance

Optimise your cluster based on how much time queries spend on different stages of processing.

- :book: [Analyzing workload performance](https://docs.aws.amazon.com/redshift/latest/mgmt/analyze-workload-performance.html)

#### :yellow_circle: Use Redshift Advance Monitoring

This GitHub project provides an advance monitoring system for Amazon Redshift that is completely serverless, based on AWS Lambda and Amazon CloudWatch. A serverless Lambda function runs on a schedule, connects to the configured Redshift cluster, and generates CloudWatch custom alarms for common possible issues.

- :wrench: [Redshift Advance Monitoring](https://github.com/awslabs/amazon-redshift-monitoring)

### Consumption

#### :yellow_circle: :new: Use Data API

Using this API, you can access Amazon Redshift data with web services–based applications, including AWS Lambda, AWS AppSync, Amazon SageMaker notebooks, and AWS Cloud9.

- :book: [Announcing Data API for Amazon Redshift
](https://aws.amazon.com/about-aws/whats-new/2020/09/announcing-data-api-for-amazon-redshift/)
- :book: [Using the Amazon Redshift Data API](https://docs.aws.amazon.com/redshift/latest/mgmt/data-api.html)

### Cluster

#### :red_circle: Increase automated snapshot retention

The default retention period of 1 day can catch organisations out in case of disaster recovery or rollback. Consider changing to 35 days. You can use the HTTP endpoint to run SQL statements without managing connections. Calls to the Data API are asynchronous.

- :book: [ModifySnapshotCopyRetentionPeriod](https://docs.aws.amazon.com/redshift/latest/APIReference/API_ModifySnapshotCopyRetentionPeriod.html)

#### :yellow_circle: :new: Use RA3 nodes

Consider using Redshift's new RA3 nodes with a mix of local cache and S3 backed elastic storage if compute requirements exceed dense compute or dense storage node levels.

- :book: [Amazon Redshift introduces RA3 nodes with managed storage enabling independent compute and storage scaling](https://aws.amazon.com/about-aws/whats-new/2019/12/amazon-redshift-announces-ra3-nodes-managed-storage/)
- :video_camera: [AWS re:Invent 2019: [NEW LAUNCH!] Amazon Redshift reimagined: RA3 and AQUA (ANT230)](https://www.youtube.com/watch?v=6pZrE_tveLI)
- :video_camera: [Amazon Redshift RA3 Nodes: Overview and How to Upgrade](https://www.youtube.com/watch?v=dSfB-zJRtJM)

#### :green_circle: Use Redshift Spectrum

Consider using Redshift Spectrum to allow users to query data straight from S3 using their Redshift cluster. This can be used in replacement of a staging schema whereby your staged data lives within your data lake and is read into Redshift via Spectrum.

- :book: [Getting started with Amazon Redshift Spectrum](https://docs.aws.amazon.com/redshift/latest/dg/c-getting-started-using-spectrum.html)
- :book: [Why you’re better off exporting your data to Redshift Spectrum, instead of Redshift](https://mixpanel.com/blog/why-youre-better-off-exporting-your-data-to-redshift-spectrum-instead-of-redshift/)
- :book: [Redshift Spectrum pricing](https://aws.amazon.com/redshift/pricing/#Redshift_Spectrum_pricing)
- :video_camera: [Cost and usage controls for Amazon Redshift](https://www.youtube.com/watch?v=CYVBHA_9_zM)

#### :green_circle: :new: Pause and resume clusters

Redshift has recently introduced the ability to pause and resume the cluster within minutes. Take advantage of this feature for non-production clusters to save money.

- :book: [Pausing and resuming clusters](https://docs.aws.amazon.com/redshift/latest/mgmt/managing-cluster-operations.html#rs-mgmt-pause-resume-cluster)
- :wrench: [Redshift Smart Pause and Resume](https://github.com/servian/aws-redshift-smart-pause-and-resume)

#### :green_circle: :new: Use elastic resize over classic resize

Consider using elastic resize over classic resize when changing both the node types and the number of nodes within your Redshift cluster. Elastic resize is much quicker (minutes vs hours) and doesn't take your cluster out of commission.

- :book: [Amazon Redshift now supports changing node types within minutes with elastic resize](https://aws.amazon.com/about-aws/whats-new/2020/04/amazon-redshift-now-supports-changing-node-types-within-minutes-with-elastic-resize/)

## Contributing

Open an issue or a pull request to suggest changes or additions.
