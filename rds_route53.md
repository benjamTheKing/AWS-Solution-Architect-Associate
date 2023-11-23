RDS
------------------
Relational database service
SQL
- postgres
- mysql
- mariadb
- oracle
- sql server
- aurora
can't ssh into rds instances

Rds auto-scaling detect and scale just set maximum
need to change SQL connection with read replicas

RDS read replicas, multi AZ
------------------
read replicas scale your read
15 read replicas in the same AZ or same region or different AZ
ASYNC replication eventually consistance
a replica can be it's own database
network cost between AZ, for managed service in the same region you're not paying that fee
but across region there is fee

RDS multi AZ disaster recovery
Sync replication to stand by, in case threre is a pb with the master there is a failover with the slave
Only 1 dns name for the two rds instance
No used for scaling the stand by instance

RDS custom
------------------
don't access to underlying system
but for oracle and microsoft sql server there is access to the ec2 instance through ssh 

Aurora
------------------
AWS technology 
- Postgres 
- Mysql
cloud optimized 
automatically grows
15 read replicas
6 copies of your data accross 3 AZ
4 copies out of 6
there is a Master, cross region

writer endpoint dns pointing to the master
reader endpoint connection load balancing

Aurora replica autoscaling
------------------
custom endpoint for certain requests
not need capacity planning 
proxy fleet 
multi-master -> multi writer
Aurora global database
5 secondary regions
less than 1 second to replicate data cross region

Aurora integrate with 
- sage maker
- comprehend 

RDS backup
------------------
automatically do a daily backup
every 5minutes 
manual db snapshot and storage it as long as you want
so stop for long time take and snapshot and restore it when you are ready cost saving

aurora automatically backup not be disabled
percona xtra backup
aurora database cloning faster than snapshot/restore for staging database


RDS Aurora security
------------------
at rest encryption
if the master not encrypted the read is not encrypted
take a snapshot and restore has an encrypted instance

in-flight encription
iam-roles to connect to database
security groups
no ssh available
cloudwatch logs

RDS Proxy
------------------
if you have a lot of connection, minimize connection, serverless, auto-scaling, multiple-AZ
force IAM connection rds proxy, accessible only in the vpc
good with 1'000 of lambda function that open connection


Elastic cache
------------------
- redis  multi AZ, read replicas set sorted set iam authentication, or redis auth, gaming leaderbord sorted sets
- memcached multi node, no high availability, pure cached distributed sasl based authentication 

in memory databases reduces load common query are cached
application stateless
retrieve the session

Database port:
- PostgreSQL: 5432
- MySQL: 3306
- Oracle RDS: 1521
- MSSQL Server: 1433


Route 53
------------------
fully manage dns, you can update the dns
domain register also
check the health or service
53 dns port

A hostname: example.com
AAAA hostname: ipv6
cname hostname to other hostname (dns to dns)

there is public and private dns record 0.5$/month
private dns in vpc

Route 53 TTL
------------------
cache this result 300s
24h ttl less traffic on route 53

CNAME vs Alias
------------------
connect load balancer to domain name
- cname point load balancer to domain name only work for the non root domain like benji.theteacher.com and not theteacher.com (not for the root domain)
- alias point hostname to aws resource app.myapp.com => blabla.aws.com, or alb .... work for root domain myapp.com ,free, native health check
  alias type A and AAAA
alias cannot set TTL, route 53 set TTL for us

Routing policy
------------------
- simple: route traffic to a single resource, multi value, pick one, can't with health check
- weighted: % go to 1 and % go to 2, health check, 
- failover: primary instance and disaster recovery, if health check is not good you have the response of the second instance
- latency based: lowest latency, health check
- geolocation: based on user location and have a default ip, restrict content
- multi-value:
- geoproximity: you have a bias, to expand traffic in a specific location (Route 53 traffic flow) schema of the us split in 2 
- ip-based: based on ip, ip-ranges

Health check: 2 loads balancers, use root 53, redirect to the 2 loads balancers, 
if 1 region is down we create health check at the route 53 level => automated failover dns
must allow incoming adresss range

Calculated health checks (have a parent that monitor health checks 256max)
health check on a private resource possible through cloud watch alarm

difference between route 53 multiple values and elb, route 53 send you address but you can change if you know another address unlike elb is force you to go in a specific instance

Ns record must be change in the 3rd party registrar if you use ionos for example


Solution architecture
------------------
golden AMI, all in 1 AMI
ebs not stateless because of 1 AZ block

Elastic Beanstalk
------------------
give a developer centric view, managed service for elb, asg, ....
full control but one interface 
beanstalk free ! 