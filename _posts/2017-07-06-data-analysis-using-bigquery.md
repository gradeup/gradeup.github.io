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

After linking Firebase project to BigQuery, Firebase automatically exports a new table to an associated BigQuery dataset every day. If you have both iOS and Android versions of your app, Firebase exports the data for each platform into a separate dataset. Each table contains the user activity and demographic data automatically captured by Firebase Analytics, along with any custom events you’re capturing in your app.
Firebase Analytics makes it easy to log custom events such as tracking item purchases or button clicks in your app. When you log an event, you pass an event name and up to 25 parameters to Firebase Analytics and it automatically tracks the number of times the event has occurred. The following query shows the number of times each event in our app has occurred on Android for a particular day:

```
SELECT
  event_dim.name,
  COUNT(event_dim.name) as event_count
FROM
  [Table]
GROUP BY
  event_dim.name
ORDER BY
  event_count DESC
```

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

### Building complex queries

What if we want to run a query across both platforms of our app over a specific date range? Since Firebase Analytics data is split into tables for each day, we can do this using BigQuery’s TABLE_DATE_RANGE function. This query returns a count of the cities users are coming from over a one week period:

```
SELECT
  user_dim.geo_info.city,
  COUNT(user_dim.geo_info.city) as city_count
FROM
TABLE_DATE_RANGE([firebase-analytics-sample-data:android_dataset.app_events_], DATE_ADD('2016-06-07', -7, 'DAY'), CURRENT_TIMESTAMP()),
TABLE_DATE_RANGE([firebase-analytics-sample-data:ios_dataset.app_events_], DATE_ADD('2016-06-07', -7, 'DAY'), CURRENT_TIMESTAMP())
GROUP BY
  user_dim.geo_info.city
ORDER BY
  city_count DESC
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

### Conversion to flat tables

We at [Gradeup](https://gradeup.co/) use Apache Airflow to run data conversion DAG's/crons which convert data from key value pairs to flat tables using queries on intraday tables.

![airflow-sample-dash](/public/img/airflow-sample-dash.png)

The following python code converts key value pair intraday table to flat one by querying the required data. It queries the intraday table for a particular event and key value and stores it to a flat table:

```
from pprint import pprint
from googleapiclient import discovery
from oauth2client.client import GoogleCredentials
from datetime import date, timedelta
import time
credentials = GoogleCredentials.get_application_default()
service = discovery.build('bigquery', 'v2', credentials=credentials)
projectId = 'PROJECT'
yesterday = date.today() - timedelta(1)
yesterday = yesterday.strftime('%Y%m%d')
job_body = {
  "configuration": {
    "query": {
      "query": """ SELECT user_dim.user_id as user_id, event_dim.params.value.string_value as post_id, DATE(USEC_TO_TIMESTAMP(event_dim.timestamp_micros)) as attempt_date, count(*) as count FROM TABLE WHERE event_dim.name ='EVENT' and event_dim.params.key = 'KEY' and user_dim.user_id is not null and user_dim.user_id != "" group by 1,2,3 """,
      "destinationTable": {
        "projectId": "PROJECT",
        "datasetId": "DATASET",
        "tableId": "TABLE"
      },
      "createDisposition": "CREATE_IF_NEEDED",
      "writeDisposition": "WRITE_APPEND",
    }
  }
}
request = service.jobs().insert(projectId=projectId, body=job_body)
response = request.execute()
pprint(response)
```

This is a sample python code which creates a DAG in Airflow using BashOperator

```
from airflow import DAG
from airflow.operators.bash_operator import BashOperator
from datetime import datetime, timedelta
import time
n=time.strftime("%Y,%m,%d")
v=datetime.strptime(n,"%Y,%m,%d")
default_args = {
    'owner': 'OWNER',
    'depends_on_past': False,
    'start_date': datetime(2017, 06, 23),
    'email': ['user@email.com'],
    'email_on_failure': True,
    'email_on_retry': False,
    'retries': 1,
    'retry_delay': timedelta(minutes=10),
}
dag = DAG('realtime_new_dag', default_args=default_args, schedule_interval='30 * * * *')
t29 = BashOperator(
    task_id='task1',
    bash_command='python file1.py',
    dag=dag)
t32 = BashOperator(
    task_id='task2',
    bash_command='python file2.py',
    dag=dag)
t32.set_upstream(t29)
t33 = BashOperator(
    task_id='task3',
    bash_command='python file3.py',
    dag=dag)
t33.set_upstream(t32)

```

### Visualizing analytics data

Now that we’ve gathered new insights from our mobile app data using the raw BigQuery export, let’s visualize it using [Redash](https://redash.io). Redash can read directly from BigQuery tables, and we can even pass it a custom query like the ones above. Redash can generate many different types of charts depending on the structure of your data, including time series, bar charts, pie charts etc.

![redash-sample-chart](/public/img/redash-sample-chart.png)

##### References

[Google BigQuery Analytics](https://docs.google.com/presentation/d/1CIPW1fHJfZWSLnjcHyOItjcFHy2ib4hlDtzLfkSS9Jc/edit#slide=id.g3f5cc2519_69_170)

[Image credits](https://image.slidesharecdn.com/opendatabigquery-distributioncopy-150918142401-lva1-app6891/95/an-indepth-look-at-google-bigquery-architecture-by-felipe-hoffa-of-google-35-638.jpg?cb=1442586368)
