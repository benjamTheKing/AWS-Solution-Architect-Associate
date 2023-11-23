VPC default
------------------
1 default vpc
3 subnets in the default vpc in different AZ
defautl route table
network acl: all open ingress and outgress
all trafic goes to internet gateway

VPC
------------------
virtual private cloud
multiple vpc in aws account

/28 (16 ip min)
/16 (65536 ip) max CIDR size on aws

vpc is private so only ipv4 private are allowed
10.0.0.0
172.16.0.0
192.168.0.0

public subnet and private subnet
first 4 IP and last IP is reserved per aws

10.0.0.0 network address
10.0.0.1-3 reserved
10.0.0.255 network address

Internet Gateway
------------------
give access to internet to your VPC
created separately from a VPC
with route tables allow internet
only 1 Internet Gateway in 1 vpc

Bastion Host
------------------
EC2 instance in a public subnet of a VPC with it's own security group and this ec2 bastion host have access to our private subnet so she can access the ec2 in the private subnet.
You connect with ssh in the EC2 bastion host and in the EC2 bastion host, you connect to the EC2 (this in the private subnet)

Bastion Host security group have to allow ssh port 22 

NAT Instance
------------------
Network address Translation
allow ec2 in private subnet to connect internet
must be launch in the public subnet 
attach IP to the NAT Instance
ec2 in the private subnet send traffic to google through nat instance IP, and NAT Instance send back traffic to ec2 private subnet
managed by you

NAT Gateway
------------------
better than NAT Instance
in a specific AZ, with an IP
can't be used in the same subnet
work with internet gateway
resilient but in the single AZ
can create multiple nat gateway in multiple AZ
no security groups
managed by aws

NACL
------------------
network access control list
firewall to the subnet level
rule have a number 1 very high
extra level of protection in front of the security group
the request go to the NACL before the security group
stateless service (outbound traffic not allowed by default)
security group is a stateful service, so the outbound traffic is allowed
NACL can block IP at the subnet level
default NACL accept inbound and outbound traffic
security group at the instance group


Ephemeral Port
------------------
used to just a connection life
random port

NACL use ephemeral port, if and EC2 in a subnet want to access to a db in another subnet

EC2: NACL outbound must open 3306(mysql port)
DB: NACL inbound must open 3306

DB: NACL outbound must allow big range of port because of NACL ephemeral port
EC2: NACL inbound must allow big range of port because of NACL ephemeral port


VPC peering
------------------
privately connect to VPC
not transitive
cross account possible
cross region

VPC Endpoints
------------------
instead of traffic of an ec2 instance go to nat gatway, internet gateway and after a service 
directly connect and used private network 

- through eni (all service)
- through gateway endpoint s3 or dynamoDB (but free) better for s3 than eni

VPC Flow Logs
------------------
capture information for IP information
vpc, subnet, eni level
connectivity 
capture information from multiple source
and put informaiton to s3, cloudwatch logs, kinesis data firehose
Athena on s3 to do analytics on vpc flow logs
cloud watch log insight also

AWS Site to Site vpn
------------------
establish, private connection in the public internet with vpn 
need two things:
    - virtual private gateway attach to the vpc
    - customer gateway (software on the datacenter)

if customer gateway have public IP used it
if have private IP and NAT used NAT IP
you have to unable route propagation on the vpc for the VPN to work

VPN cloudhub for multiple datacenter that connect to aws and also direct connect

Direct connect (DX)
------------------
dedicated private connection from a remote network to vpc
need:
     - virtual private gateway attach to the vpc
faster because through private connection, lower cost

AWS direct connect location (physical place)

direct connect gateway if you want to connect to multiple VPC
1 month to establish 
good to encrypt (because unencrypt)

can have two physical locations, to maximise two link to the cloud 

direct connect + vpn site to site

Transit Gateway
------------------
connect multiple vpc through transite gateway without vpc-peering
IP MULTICAST= transit gateway
ecmp two tunnels

Traffic mirroring
------------------
capture and inspect network traffic
like a sniffer

Egress-only Internet Gateway (ipv6)
------------------
it's nat gateway for ipv6

Network firewall
------------------
at the vpc level
control all the traffic


VPC Sharing: 1 vpc share multiple subnet
if you have public ip in vpc, the NAT is making by the internet gateway
if you have private ip, it's the NAT instance or the NAT gateway 

private link is a user service 
you will need to create interface type VPC