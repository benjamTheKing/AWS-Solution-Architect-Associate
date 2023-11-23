CloudFront
------------------
content delivery network CDN
our content is propagated all around the world
improve the read performance at the edge location (bord)
lower latency
cache 

dynamic content or static content


CloudFront Origin:
- S3 bucket
    - origin access control to guarantee that only cloudFront access to your bucket
    - cloudfront can send data to S3 bucket (ingress)
- HTTP 
    - load balancer, ec2, S3 website, http backend
        - public ec2 instance communicate with edge location, no privace vpc connectivity, security group allow range of ip cloudFront, same for alb, but the ec2 can be private

Difference between cloudfront and s3 replication cross region? 
- CloudFront: edge location but everywhere (for a day) if static content (cache content)
- S3 cross region you have to choose region, no caching, dynamic content (replicate entire bucket)

Geo restriction:
restrict based on country; allow list, or block list, 

Pricing: 
cost varies
reduce number of edge location

All class
200 regions
100 regions

Cache invalidation:
based on TTL, can force cache refresh --> cloudFront Invalidation

cloud front doesn't belong to a vpc and work from edge location


AWS global accelerator
------------------
Anycast IP: two servers have got the same IP, the server go to the closest server

global accelerator use anycast IP, with help of edge location 
what do you mean about anycast ? 

- provide two static global IP 
- simplify traffic management
- bring traffic to multiple region (vs elb in only one region)


health checks 
internal AWS network
2 external IP to whitelist, ddos protection

dns caching is good with global accelerator

support udp or tcp

Difference between global accelerator and cloudfront ? 

Both used the edge location and global network, shield,

- cloudfront improve performance dynamic, serve in the edge location cache content
- global accelerator improve but at the origin, proxied UDP, MQTT, static IP, vpn

cloudfront better than global accelerator for application resiliency

Decoupling application
------------------

2 pattern:
    - synchronous application buying service --> shipping service
    - asynchronous application event based called queue buying service --> Queue --> shipping service

if 1 service suddenly spikes it can have problem. 

SQS: queue model
SNS: pub/sub model
Kinesis: real-time streaming model (big data)


SQS
------------------
Queue: 

Producer send message to Queue, one or multiple producer
Comsumer pull message of the Queue, and delete from the queue

Standard queue, decouple application 
unlimited, no limit
each message is short-lived: 14 days max  1 day is okay
each message is less then 256kb 
it's possible to have duplicated message

consumer: ec2, on prem, lambda, asg can scale up with cloudwatch alarm from metric queue length
up to 10 messages at a time

sqs access policies to control access
SQS FIFO 300 message per second, if you activate batch mode for example 4 batch you have 1200 message per second (max 10 batch)

delay queue to postpone (reporter) the delivery of new message


SQS Message visibility timeout 
------------------
after a message is pull it become invisible by other consumer
30s by default
can change message visibility timeout

Long polling: wait and pull for 20s and just wait, decrise number of API call 1 to 20s, optimize API call
FIFO: first in firt out, ordered in the queue

SQS with ASG: 

SQS not with Kinesis data stream


SNS
------------------
1 message and many receiver
SNS topic
Publisher, subscriber
send email, notification, endpoint, sqs send message to a queue

subscriber: sqs, lambda kinesis data firehose, sms, email, http endpoint
publishers: S3 bucket, cloudwatch, asg, lambda, dynamo db ...

SNS real link with kinesis data firehose, to put to s3
FIFO 3000
message filtering

not for real time application


Kinesis
------------------
big data, collect, process, analyse streaming data
- kinesis data stream: ingest, capture process store data
- kinesis data firehose: load data into aws data stores
- kinesis data analytics: analitics through sql and flink
- kinesis video stream: capture store video

Kinesis data stream
------------------
enhanced fanhout to improve number of consumer
real time
stream big data into your system
ordering of record


Producers send data to kinesis data stream: kinesis agent, application 
send a record 1MB/sec per shard, or 1000msg/sec per shard

can replay in case of failure 

Consumers: 
lambda, application, kinesis data firehose, kinesis data analytics
receive a record and the partition key
2MB/sec, or 2MB/sec per share

365 days and add partition key

provisioned mode or on demand mode
deploy in a region

can access kinesis data stream with ec2 with vpc endpoints directly in aws

cloudtrail for monitoring

add more shard if it's not enough

Kinesis data firehose
------------------
Take data from producer kinesis data streams, cloudwatch, aws iot, application
can transform data with lambda function

kinesis data firehose -> aws destination(S3, redshift{copy from S3}, amazon opensearch)
                      -> 3rd party (splunk, datadog)
                      -> own API
        -> failed data can go to S3 backup
near real time 1min minimun

kinesis data firehose good with lambda and s3


data order
------------------
if you want dynamic number of group better with SQS FIFO Group ID, if you have 10k trucks kinesis data stream is better
but if you want a lof of consumer FIFO is better because number of shard limited in k-d-s

ECS
------------------
ECS tasks on ecs cluster with EC2 

EC2 instance profile 

ECS task role 

With EFS compatible with ECS and Fargate

Fargate+EFS good 

S3 impossible with container

ECS with cloudwatch alarm

sqs with ecs

APP Runner
------------------
easy to deploy api, web app

AWS Lambda
------------------
15min of run max
pay per request, and compute time

Lambda Container Image: ECS/Fargate is better ! 
limit per region: 128mb to 10Gb, 15minutes, 4kb env var, tmp space/10Gb

Lambda @edge attach to cloudfront, close to user python nodejs Thousands/s
CloudFront function js Millions/s

lambda outside of my vpc
dynamoDB is public
but lambda can create ENI: Elastic network interface in our subnets

Lambda with RDS Proxy to 
RDS Proxy never be public
RDS invoke lambda function allow outbound traffic, and have the required permission
RDS notification when there is event at the database level, snapshot, version but not information about data in the database

There is lambda with container
lambda is in AWS-owned VPC 

lambda limit 1000, you can request aws support to increase


Dynamo DB
------------------
database accross multiple AZ
No SQL massive workloads
integrated with IAM
Standard and IA Standard
made of tables
primary key
400kb max size of an item
the schema need to rapidly evolve
RCU read WCU write provisioned mode (decoupled RCU from WCU)

point in time recovery with DB

DAX accelerator
------------------
if lot of reads, micro second latency
dax cluster to cache
TTL 5min
on top of amazon db, dax is better than elasticache, elasticache store aggregation result

dynamo db streams like kinesis data streams and after lambda

enable dynamo db streams before dynamo db global tables

dynamo db global tables prerequisites streams

TTL and delete (web session in dynamo DB) 

DynamoDB for Backup recovery

DynamoDB and S3 export table to S3 (PITR) if you want some queries with athena
dynamoDb just export a table to S3
import from S3 to DynamoDB


API Gateway
------------------
serverless to create REST API (public) proxy
proxy request to Lambda for example
handle websocket
dev,test,prod
authentication and authorization
api keys, swagger

API Gateway on top of HTTP Endpoint or Lambda
on top of Kinesis data streams

Cloud front on top of api gateway, api gateway is in one region but cloudfront all around the world

private API gateway using VPC endpoint
IAM
caching the lambda function at the API level



Step functions
------------------
design application with a graph 
human approval 


Cognito
------------------

User pools: for app users sign in api gateway and alb
Identity pools: provide aws credentials to users so they can access aws resource directly federated identity 

cognito user pools: identity provider 

cognito, web or mobile user and authentification SAML 
cognito serverless technologies

cognito integrate with API Gateway
cognito can generate temp credentials with aws sts to provide access to mobile user

cognito identity: provide aws credentials to use other aws service to your user

cognito user pool: built-in user management

ServerLess architecture
------------------

Cloudfront + S3 with Origin Access Control of cloudFront

REST HTTPS API Gateway + Lambda(read) + Dax + DynamoDB (global tables)

For Emails: Dynamo DB Streams invoke Lambda function with iam role to AWS SES simple email service

S3 transfer acceleration + lambda + S3 + SQS 


Which db ? 
------------------
- rds or aurora (joins)
- noSQL (flexible) 
    - dynamo db (json)
    - Elasti cache (key/value)
    - Neptume (graph)
    - Document DB (mongoDB)
    - Keyspaces (cassandra)
- object store
    - S3 (big object)
    - Glacier (backup/archives)
- data warehouse
    - Redshift (olap)
    - Athena
    - EMR
- Search
    - openSearch(json)
- Graphs
    - neptune
- Timestream

RDS
------------------
manage postgre, mysql, oracle, sqlserver, mariadb
provision
autoscaling
read replicas
multi az
automated backup 
manuel db snapshot
backup 35 days
rds custom to have access to the underlying instance (oracle, sql server)
sql and transactions

rds multi-az synchronous replication
rds read-replicas asynchronous replication

Aurora
------------------
postgre and mysql
data in 6 replicas and 3 AZ
auto scaling 
read-replicas
writer endpoint
reader endpoint

aurora serverless
aurora multi-master can write
aurora global 16 reads in each region < 1 second storage replication
aurora machine learning sage maker and comprehend
aurora database cloning for staging faster than snapshot  
better than rds 

Elasticache
------------------
manage redis or memcached
in memory datastore submillisecond
redis (clustering) read replicas
redis authentification
code changes mandatory
key/value store
many read 
session data
Not SQL

Dynamo DB
------------------
manage serverless
millisecond latency
provision (autoscaling)
on demand 
can store session data and with ttl
read and write decouples
read cache Dax cluster microsecond read latency 
event processing dynamo db streams
dynamo db global table active active setup
automated backup or on demand
export S3
rapidly evolve schema, serverless application 400kb max

S3 
------------------
5TB
Tiers lifecycle policy
S3 batch encrypt object
big files

Document DB
------------------
Mongo DB no SQL
index json data

Neptume
------------------
graph database
social media
wikipedia

Keyspaces
------------------
cassandra cql

QLDB
------------------
financial transaction ledger
immutable system
journal
hash

Timestreams
------------------
time series database

Athena
------------------
Serverless query service to analyse data in S3 through SQL
can query anywhere with a data source connection (lambda) and store result to S3
csv, json ...
pay per TB 

Amazon QuickSight connect to Athena to S3 

columnar data for cost staving

larger file easy to scan

Glue
------------------
ETL job
extract transform load to redshift
glue job bookmarks to prevent old data to not reprocessing
transform data to parquet format

Redshift
------------------
Postgre not OLTP
OLAP : online analytical processing
it's data warehouse
collumnar storage of data
SQL to query

enhanced vpc routing to copy and unload traffic

Redshift you have to load the data before SQL, if loaded must query than athena
Redshift build indexes, join aggregate

leader node and computer node

provisioned node 

Redshift can automaticly take snapshot and move to another region to disaster recovery

Way to insert data to Redshift
    - kinesis data firehose (through s3)
    - S3 (copy command in redshift)
    - ec2 

Redshift spectrum: not load data, keep data in s3, and cluster redshift perform the query

Amazon Opensearch
------------------
search field, analytics and complement of a database
centraly store data and logs
managed or serverless
can enable SQL

Opensearch dashboard

EMR
------------------
big data on aws
hadoop
cluster
spark

Quicksight
------------------
serverless machine learning to create dashboard interactive
great with redshift

Amazon data lake
------------------
data lake = central place to have all your data for analytics purposes
centralize permission of access to the data

Kinesis data analytics
------------------
- for sql
    - kinesis data stream
    - kinesis data firehose
to kinesis data stream -> lambda
to kinesis data firehose -> s3
                         -> Redshift
s3 can enrich data
time-series analytics
real-time dashboard
real-time metrics

- for apache flink
    - java, scala, sql
from 
   - kinesis data stream
   - amazon msk

Amazon MSK
------------------
kafka to stream data
msk serverless

Snowball
------------------
copy data from outside to aws, not from aws region to another aws region
can't copy data directly to glacier

aws datasync
------------------
to transfer data
every 24h

FSX for lustr
------------------
hot data 
cold data parralel
high performance only because otherwise it's very expensive

aws data migration service 
------------------
transfer data between s3 and kinesis


datasync: for synchronisation (automate and accelerate)
dms: for migration (one time)

AWS Transfer family: SFTP, FTP, FTPS not support windows server


CloudFormation stackset to deploy the same infrastructure in another region