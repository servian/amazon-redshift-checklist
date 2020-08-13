<h1 align="center">Amazon Redshift Checklist</h1>

<h4 align="center">The Amazon Redshift Checklist is an exhaustive list of all elements you need to have / to test before launching your Redshift cluster into production.</h4>

<p align="center">
  <a href="#how-to-use">How To Use</a  • <a href="#contributing">Contributing</a>
</p>
<!-- <p align="center">
    <span>Other Checklists:</span>
    <br>
  <a href="https://github.com/thedaviddias/Front-End-Performance-Checklist#---------front-end-performance-checklist-">🎮 Front-End Performance Checklist</a  • <a href="https://github.com/thedaviddias/Front-End-Design-Checklist#front-end-design-checklist">💎 Front-End Design Checklist</a>
</p  -->

## Table of Contents

1. [Designing Tables](#designing-tables)
2. [Loading Data](#loading-data)
3. [Performance](#performance)

---

## How to use?

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

- [ ] **Choosing a Data Distribution Style:** :bangbang: In order to utilise the parallel nature of Redshift, data must be correctly distributed within each table of the cluster.

  - :book: [Choosing a data distribution style](https://docs.aws.amazon.com/redshift/latest/dg/t_Distributing_data.html)

- [ ] **Choosing a Column Compression Type:** :interrobang: Ensures data is better compressed utilising less storage space.

  - :book: [Choosing a column compression type](https://docs.aws.amazon.com/redshift/latest/dg/t_Compressing_data_on_disk.html)

- [ ] **Choosing Sort Keys:** :interrobang: Ensures data is retrieved from within each node in the most performant way.

  - :book: [Choosing sort keys](https://docs.aws.amazon.com/redshift/latest/dg/t_Sorting_data.html)

- [ ] **Defining Constraints:** :o: Not enforced by Redshift but helps query optimisers.

  - :book: [Defining constraints](https://docs.aws.amazon.com/redshift/latest/dg/t_Defining_constraints.html)

**[:arrow_up: back to top](#table-of-contents)**

---

## Loading Data

- [ ] **Use a COPY command to load data from S3:** :bangbang: Load files from S3 to Redshift via COPY to ensure parallel quick loads.

  - :book: [Use a COPY command to load data](https://docs.aws.amazon.com/redshift/latest/dg/c_best-practices-use-copy.html)

- [ ] **Compress your data files:** :interrobang: Use either GZIP, LZOP, BZIP2, or ZSTD.

  - :book: [Compress your data files](https://docs.aws.amazon.com/redshift/latest/dg/c_best-practices-compress-data-files.html)

- [ ] **Use a Multi-Row Insert:** :interrobang: If a COPY command is not an option and you require SQL inserts, use a multi-row insert whenever possible.

  - :book: [Use a multi-row insert](https://docs.aws.amazon.com/redshift/latest/dg/c_best-practices-multi-row-inserts.html)

- [ ] **Load data in Sort Key Order:** :interrobang: Load your data in sort key order to avoid needing to vacuum.

  - :book: [Load data in sort key order](https://docs.aws.amazon.com/redshift/latest/dg/c_best-practices-sort-key-order.html)

- [ ] **Loading tables with Automatic Compression:** :interrobang: Use the COPY command with COMPUPDATE set to ON to automatically set column encoding for brand new tables

  - :book: [Loading tables with automatic compression](https://docs.aws.amazon.com/redshift/latest/dg/c_Loading_tables_auto_compress.html)

- [ ] **Split your load data into multiple files:** :o: Split your load data files so that the files are about equal size, between 1 MB and 1 GB after compression.

  - :book: [Split your load data into multiple files](https://docs.aws.amazon.com/redshift/latest/dg/c_best-practices-use-multiple-files.html)

**[:arrow_up: back to top](#table-of-contents)**

---

## Performance

- [ ] **Turn on Automatic WLM:** :bangbang: Amazon Redshift determines how many concurrent queries and how much memory is allocated to each dispatched query.

  - :book: [Implementing workload management](https://docs.aws.amazon.com/redshift/latest/dg/cm-c-implementing-workload-management.html)

- [ ] **Enable Concurrency Scaling:** :interrobang: Dynamically adds concurrent clusters improving read query concurrency.

  - :book: [Working with concurrency scaling](https://docs.aws.amazon.com/redshift/latest/dg/concurrency-scaling.html)

- [ ] **:new: Use AZ64 column compression encoding:** :interrobang: Consider using Redshift's proprietery new column encoding algorithm AZ64.

  - :book: [Amazon Redshift introduces AZ64, a new compression encoding for optimized storage and high query performance](https://aws.amazon.com/about-aws/whats-new/2019/10/amazon-redshift-introduces-az64-a-new-compression-encoding-for-optimized-storage-and-high-query-performance/)

- [ ] **Loading tables with Automatic Compression:** :o: Use the COPY command with COMPUPDATE set to ON. Note this will impact performance and should only be used on the first load.

  - :book: [ANALYZE COMPRESSION](https://docs.aws.amazon.com/redshift/latest/dg/r_ANALYZE_COMPRESSION.html)

- [ ] **Analyse query performance:** :o: `STL_ALERT_EVENT_LOG` table allows users to analyse and improve performance issues.

  - :book: [STL_ALERT_EVENT_LOG](https://docs.amazonaws.cn/en_us/redshift/latest/dg/r_STL_ALERT_EVENT_LOG.html)

**[:arrow_up: back to top](#table-of-contents)**

---

## Contributing

Open an issue or a pull request to suggest changes or additions.

**[:arrow_up: back to top](#table-of-contents)**

[low_img]: data/images/priority/low.svg
[medium_img]: data/images/priority/medium.svg
[high_img]: data/images/priority/high.svg
