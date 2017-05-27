---
layout: post
title: Gradeup Engineering
---

Gradeup's mission is to make learning process significantly more collaborative and engaging, and hence more impactful. To make that possible we leverage some of the best open source tools and technologies. Then we bundle it up neatly as a platform that enables learners to interact and learn with each other.

We consistently opt for promising new technologies that would deliver an awesome experience over more mature alternatives.

Our architecture looks like

![techstack](/public/img/techstack.png)

### NodeJS

Our whole backend is based on NodeJS. We use NodeJS version 6.9.

### Cassandra

We use Cassandra for storing scalable data. Most of our social features are powered by it.

### ElasticSearch

ElasticSearch is the query frontend. It is used to power our feed and search.

### Mysql

We use MySQL as a hosted service from Amazon, namely RDS. It generally stores data which is very small scale and powers our business.

### Memcached

It is used for storing all JSON related data.

### Redis

It acts as a caching backend and stores our session related data.

### CloudFlare

We use Cloudflare as DNS service along with its awesome CDN capabilities. Images stored on S3 are served by Cloudflare CDN. It also acts as a firewall and prevents our server from unauthorized access.

### RabbitMQ

We use RabbitMQ for powering all our queueing related jobs. NodeJS workers along with RabbitMQ powers our many services like notification service.

