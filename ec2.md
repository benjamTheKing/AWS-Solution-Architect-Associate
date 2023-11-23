EC2 Fundamentals
------------------

AWS Billing
------------------
Aws Budget to create a budget and set an alarm

EC2: elastic compute cloud
------------------
- Os: 
- CPU: 
- RAM: 
- Storage space: 
- Network:
- Firewall rules:
- Bootscrap script:

EC2 Bootscrap script
------------------
Script run at the boot time EC2 user data
with privilege sudo 
only at the boot of the instance

EC2 informations
------------------
EC2 Key pair to access to the instance through ssh 
.pem default format
By default, delete storage on termination
IP changes at each restart

EC2 instances type
------------------

m5.2xlarge
m=class
5=generation
2xlarge= size(memory, cpu)

7 types:
- general purpose (diversity of service, balance between memory, cpu, networking)
- compute optimized (bash processing, hpc, machine learning, gaming, hig level of cpu) C class
- memory optimized (large data, high level databases, cache, in memory database, real time processing) R class
- storage optimized (high frequency of read and write access) I class (oltp)

use elastic fabric adapter if you want to maximise network performance

Security groups
------------------
security groups controll trafic at the EC2 level
controls traffic of the ec2, by api or rules
inbound and outbound traffic
lockdown to region/vpc
you can connect security group with another security group
3389 rdp port for windows (like ssh)

EC2 instance connect
------------------
browsers session to connect like ssh to the ec2 instance

EC2 instance roles
------------------
used iam role for ec2 and without access key 



Ec2 instance options
------------------
- on demand pay by second
    - after the first minute (short term but uninteruppted workloads)
- reserved (1 or 3 years)
    - 72% discount compared to on demand, you reserve os, reservation period, pay upfront (database)
    - convertible reserved instance, if you want to change your instance
- saving plans (1 or 3 years), you assign amount of dollar to purshace instance
    - discount based on reserved instance, i want to spend 10€/h lock for region for m5, and AZ 
- spot instance very cheap very short, can lose instance
    - 90% discount, resilient to failure, distribute workload
- dedicated hosts (entire physical)
    - physical server with ec2, compliance requirements, or software license , access to the physical instance
- dedicated instance
    - share hardware with your account, mean own instance 
- capacity reservation in AZ 
    - for any duration, change at any time, no discount

Spot instances
------------------
define a max spot price, current spot price < max spot price 
2 minutes grace period
spot block (block between 6h at the spot price)
persistent request if your instance get stopped, if the price down aws restart your instance
if you terminate the persistent request it's not terminate spot instances and if you stop instance, the persistent request will relaunch instance

Spot fleet
------------------
Spot fleet choose the most appropriate fleet of instance, you define a strategie: 
    - lowest price
    - diversified 
    - capacity optimized
    - priceCapacity optimized
Spot fleet= spot instance + on demand instance

Ec2 Architect level
------------------
Elastic IP: IPv4
Maximun 5 Elastic IP
no use elastic ip, instead use range of IP and register a DNS name to it, and load balancer

EC2 placement group
------------------
Define strategie
    - cluster (low-latency) in 1 AZ
        - same hardware, same AZ, super low-latency, if there is a failure all instance fail at the same time
        - big data job to complete fast
        - highest network 
    - spread (spread instance accross hardware max 7 per group per AZ) critical application
        - minimize failure, example multiple AZ, high availability
    - partition spread accross many partition 100s of bigdata EC2 Hadoop cassandre, kafka, distributed safe from rack failure multiple AZ


ENI elastic network interface
------------------
virtual network card, access to the network, but outside of EC2, provide network connectivity (private IP)
Ec2 can have 2-3 ENI, one Public IPV4, mac adress
Can create ENI independently of EC2, ENI is bound to a specific AZ
can move ENI from an EC2 to another (move ip from EC2 to another)

EC2 hibernate
------------------
if stop, ebs volume will be kept.
Memory preserved
Ram dumped into EBS volume and loaded in the EC2 when you restart the instance
must be encrypted
Boot up fast or save Ram 
RAM sized 150gb certain OS, instance family, no more than 60 days


EBS volume
------------------
Elastic block store, network drive you can attach to instance while running
one instance at a time
bound to specific AZ
USB stick can move to another EC2
have a provisioned capacity, can increase capacity
can do snapshot to move to another AZ
2 usb stick is ok for an EC2 instance
EBS can be unattached
Delete on termination in EC2 by default
Long term


EBS snapshot
------------------
Backup of an EBS volume, recommended to dettached from EC2 before snapshot
Transfer ebs from AZ to another

- EBS snapshot Archize 75% cheaper 24 72h to restore snapshot
- Recycle bin for EBS snapshot to retain deleted snapshot 1 day to 1 year
- Fast snapshot restore  no latency to restore snapshot but very expensive

AMI Amazon machine image
------------------
Customization of an EC2 instance, own software, faster boot
For a specific region
- public AMI (AWS)
- own AMI (maintain yourself)
- AWS marketplace AMI (sold by someone)
ami is based on snapshot so when you create an AMI you have an underlying snapshot

EC2 instance store
------------------
Even higher than EBS, 
High performance hardware disk attach to EC2
Better I/O performance
if you stopped the instance, the storage will be lost
Not use as a durable storage, 
uses cases: buffer/cache/temporary content
Backup please 



EBS volume type
------------------

- GP2/GP3 balance price and performance      (EC2)
    - low latency, 1 Gib - 16 TiB
    max iops 16 000
- IO1/IO2 critical low latency high workload (EC2)
    - critical operation, database workloads gp2 ---> io2
    max iops 64 000

not use this storage at the boot 
- HDD st 1 low cost frequently access (cannot be a boot volume for EC2)   Throughput optimized big data, warehousing 
- cold HDD sc 1 lowest cost less frequency access archive data

EBS multi attach only IO2 and IO3
------------------
EBS attach to two instances or more at a time in one AZ
read and write at the same time
16 EC2 instances at a time

EBS encryption
------------------
keys from KMS
create ebs snapshot volume
encrypt the snapshot using copy
create new ebs volume from encrypted snapshot (volume will be encrypted)

EFS
------------------
network file systems, mountain on many EC2 in different AZ
expensive 3*gp2
highly available, scalable
NFS protocol
Linux AMI only 
POSIX file system
don't need to provide capacity in advance

EBS vs EFS
------------------
EBS: 1 instance lock at AZ level, to migrate you need to take snapshot, terminated by default
EFS: attach to 100 instances in different AZ, only for Linux (POSIX), EFS-IA for cost saving
EBS instance store physically attach for database  iops of 310 000

Vertical scalability
------------------
increase size of instance (rds for example)
scale up

Horizontal scalability
------------------
increase number of instance (distributed system)
scale out

High availability
------------------
2 AZ, survives a data loss

Read replicas
------------------
High scalability

ELB elastic load balancer
------------------
- single point of access(dns)
- spread load accross multiple downstream instance
- handle failure (healthchecks)
- ssl termination
- stickiness (cookies)
- /health

- clb: classic deprecated
- alb: http, https, websocket
- nlb: netword: tcp, tls, udp
- gatewaylb: network protocol 3 -> IP

security group allow only ELB 

ALB
------------------
layer 7 http
route to multiple http application (target group)
redirect http -> https load balancer level
routing based on path, hostname, query string, headers

ALB great with container ECS port mapping to redirect

managed by auto-scaling group or ecs task, lambda function

the true ip of an EC2 instance is in the header X-FORWARDED-FOR

NLB
------------------
layer 4
tcp and udp traffic 
handle millions of request per second extreme performance
one static IP per AZ 
NLB provide dns and static ip
TCP and HTTP for health checks

GLB
------------------
layer 3 network level IP packet
inspect the traffic before to go to an application
Gateway load balancer ---> 3rd Party application --> Application
geneve protocol  port 6081

Sticky session
------------------
client make 2 requests but have the same instance and not spread the request thanks to cookie
NLB, ALB, CLB
imbalance load 
AWSALBAPP, AWSALBSG, AWSALB (cookie name to not use)

Cross zone load balancing
------------------
without cross zone load balancing
AZ1 10 instances   50% traffic
AZ2 100 instances  50% traffic
with cross zone load balancing
AZ1 10 instances   1/110% traffic per instance
AZ2 100 instances  1/110% traffic per instance

enabled by default with alb (target group level)
nlb glb disabled you have to pay 
clb disabled

SSL certificates
------------------
there is http between load balancer and ec2 instance
SNI: solve the problem of loading multiple SSL certificate onto one web server
only on ALB, NLB
The alb have 2 certificates and 2 targets groups and he redirects user to the good target groups and load the good certificate

Connection Draining Deregistration delay
------------------
give some time to complete in-flight requests while the instance is unhealthy
user drain to another instance and stop to redirect to this instance


ASG
------------------
scale out add ec2 instance
free
elb send to asg that one instance is unhealthy
launch template (to provision)
scale based on cloudwatch alarms

Scaling policies
------------------
- Target tracking policies
- Simple scaling (cloudwatch alarms)
- Schedule action (black friday) increase min capacity
- Predictive scaling (anticipate based on old traffic) Machine Learning power
- Scaling cooldown 5minutes not stop nothing


EC2 launching template
------------------
global term, create a global template to launch configuration

EC2 launching configuration
------------------
cannot use to provision capacity 

you can put an instance to the StandBy status in an ASG 

you can change tenancy of an EC2 from dedicated to host and from host to dedicated
