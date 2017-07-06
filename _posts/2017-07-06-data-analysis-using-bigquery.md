---
layout: post
title: Data Analysis using BigQuery
---

At Gradeup, we organize weekly engineering sessions, where in our whole engineering team shares their experiences, rants about some technology or having a knowledge session over some topic.

This week, the topic for our session was __Data Analysis using BigQuery__ being organized by Prabhjot Singh [@prabh-me](https://github.com/prabh-me).

A brief summary of the talk is as follows

## What is BigQuery

BigQuery is Google Cloud Platform’s interactive big data service. It is used for analyzing terabytes of data in a matter of seconds through SQL-like queries. It provides fast, interactive analysis with a familiar language, SQL. It is built on [Google Dremel](https://research.google.com/pubs/pub36632.html).

### Fundamental concepts for Dremel:

* Large table scans are highly parallelizable.

* Data is stored in a columnar-based format.


### What happens when a query is received ?

When a query from the client is received at the root server, the workload is divided and distributed to a number of intermediate servers, which is then further reduced to the leaf servers. The leaf servers read from the columnar-based storage. The data is then sent to the intermediate machines and then final result is aggregated on root machine.

Below is a diagram which explains this

![bigquery-architecture](/public/img/bigquery-architecture.jpg)

### BigQuery Pricing

* Cost (model: pay for what you use):

* Queries: $0.005/GB ($5/TB).

* Storage: $0.026/GB ($26/TB).

* Freebie: 1st TB of queries per month is free.

### Linking BigQuery with Firebase

We can dump firebase analytics data directly to Bigquery which is stored in key values format, with keys being user_dim and event_dim. For more details one can follow this [link](https://cloud.google.com/solutions/mobile/mobile-firebase-analytics-big-query)

### Sample Queries

* Query to display count of users that have performed a certain event with a particular key and value

```
SELECT
user_dim.user_id, count(*)
FROM
['<your_table_name>’]
where event_dim.name = '<your_event_name>'
and event_dim.params.key = '<your_key>'
and event_dim.params.value.string_value = '<your_value>'
group by 1
```

We use `Flatten` function in case we need to join two queries that process data from flatten table in case a repeated field is encountered in both the queries:

* Query using join and flatten function

```
select B.event_dim.params.value.string_value from
(SELECT
event_dim.timestamp_micros
FROM
  FLATTEN(FLATTEN(<your_table_1>,event_dim.name), event_dim.params)
where event_dim.name = '<your_event_name>'
and event_dim.params.key = '<your_value>'
) A inner join
(
SELECT
event_dim.timestamp_micros
FROM
  FLATTEN(FLATTEN(<your_table_2>,event_dim.name), event_dim.params)
where event_dim.name = '<your_event_name>'
) B
and A.event_dim.timestamp_micros=B.event_dim.timestamp_micros
group by 1
```


##### References

[Google BigQuery Analytics](https://docs.google.com/presentation/d/1CIPW1fHJfZWSLnjcHyOItjcFHy2ib4hlDtzLfkSS9Jc/edit#slide=id.g3f5cc2519_69_170)

[Image credits](https://image.slidesharecdn.com/opendatabigquery-distributioncopy-150918142401-lva1-app6891/95/an-indepth-look-at-google-bigquery-architecture-by-felipe-hoffa-of-google-35-638.jpg?cb=1442586368)
