IAM user & groups:
=========
IAM is a global service
you shoudn't used root account, unstead create users, groups
group contains only users
user doesn't have to belong to a group
user can belongs to multiple group
user can have multiple policies attach to him

IAM permission:
=========
Through json document allow user to use some service
You don't allow everyone to use everything
Least privilege principle


IAM Policies
------------
Inline policie belong to one user 

IAM Policie example
------------
```
{
 "Version": "2012-10-17",
 "Id": "S3-Account-Permissions",
 "Statement":[
    {
        "Sid": "1",
        "Effect": "Allow",
        "Principal":{
            "Aws":["arn:aws:129873:root"]
        },
        "Action":[
            "s3:GetObject"
        ],
        "Resource":["arn:aws...:mybucker/*"]
    }
 ]
}
```

IAM Policies all get
--------------
'iam:Get*'
Version doesn't exist in policy keyword ! 

IAM Password policy
------------
set up minimun password length ...
allow have to change password, every month ... to prevent attack brute force
mfa to prevent all iam user: password you know and device your own 
MFA options: 
 - Virtual device
 - Physical device (Yubikey)
 - Hardware key (gemalto)


How user can access to aws ? 
----------------
- Console
- CLI (command line interface)
- SDK (software developement kit)

Access Keys with secrets keys allow me to access AWS API ! 

How to configure the cli ? and test if it's work

```
aws configure 
aws iam list-users
```

CloudShell
-------

it's a terminal in the cloud, free to use and already configure with aws and your credentials.
File stay in Cloudshell even after refresh the page
Download and update file in CloudShell


IAM roles
------------------
it's for service and not user ! 
for example role to a ec2 to performs action on aws, EC2+iam role can access to service


IAM Security
------------------

IAM Credentials Report: list all account user and status of credentials
IAM Access Advisor: show the service permission access to a user, see which permissions used 

Organizations
------------------
global service
allow manage multiple aws account
main account is management account
and other are member account 1 organization per member
pricing benefit because sum up, 
share reserve instances and saving plans
api to create account 

Root organization unit
OU dev
OU finance
    Ou prod

better security
central S3 account
service control policie (iam polici apply to OU)

management full admin power even if scp policie
1 explicit deny on menber it's enough to forbid

IAM Role or Resource based policy
------------------
both valid to s3 for example
the difference is that the role have other permission instead the rbp is only allow s3 bucket
some resource doesn't have resource based policy such as kinesis stream, ecs so use iam role

sns, sqs, lambda support rbp

IAM boundaries (frontières in french)
------------------
iam boundarie can block iam policy
explicit deny = deny

IAM identity center
------------------
sso

AWS directory service
------------------
AD Connector just as a proxy to on premise AD
AWS Managed AD  own AD on AWS
Simple AD cannot be join with on premise AD

we can connect AD connector and AWS Managed AD with on prem AD and IAM Center

AWS Control Tower
------------------
set up and govern a secure and compliant multi account aws environnement,
uses Organization

automate environnement

guardrails on organization
preventive (scp) or detective (config)

KMS
------------------
KMS Key policies to control KMS CMK key

have to configure permission for source KMS key kms:Decrypt and for target KMS key kms:Encrypt to used S3 Replication Service

able to audit key with cloudTrail
integrated with many services
never store in code 

multi-region key, primary key, in region 1, and the key can copy in other regions
the key-id is the same 
it's not global
not recommanded to use it (global system)

only use with dynamodb global/ global aurora to encrypt data in a specific region 
and decrypt content to an other region 

AMI sharing with KMS

SSM Parameter store
------------------
secure storage for parameter configuration and secret 
parameter policy


AWS secret manager
------------------
storing secret, capability to force rotation of secrets
using Lambda
good integrated with rds, 
using KMS

secret for aurora, secret manager

multi-region secret replicate secret

AWS Certificate Manager
------------------
deploy tls certificate
cannot create public certificate for EC2

daily expiration event bridge from ACM to invoke SNS notification

how to request ACM ?
domain name
validation method (dns or email)

API Gateway + cloudfront (all cloudfront certificate are in us-east-1)

aws certificate manager can renew automaticly certificate


WAF
------------------
block on layer 7 
not possible on nlb
web access control list
waf on the same region of the alb

global accelerator to have 1 ip
can block ip 
can block region (geographic block)


Shield
------------------
DDOS attack
distributed deny of service
shield advanced support and reimbursment in case of trouble 
have to enable consolidate billing if there is multiple account using shield advanced support
shield provide security also to instance outside of aws


Firewall manager
------------------
with organization, manage rule
set security policy 
waf rules
shield rules
security group
network firewall

All firewall in one place

Guard Duty
------------------
Intelligent threat
ML algorithm to fine anomaly detection
looks at cloudTrail, vpc flow logs, dns logs
malicious activity

guard duty protect you against cryptocurrency attack

AWS Inspector
------------------
automated security assessement
for 
    - ec2
    - ecr
    - lambda
scan infrastructure
like os problem
inspector is used to check vulnerabilities on ec2

Macie
------------------
Data security
alert you to sensitive data and notify you with eventbridge

SCP (Service control policy)
------------------
one type of policy to manage your organisation
central control for permission in an organisation
can restric which AWS services, resource, and individual API, the user can access
you can define condition

- scp have priority against IAM permission policy
- scp affect also root account
- scp doesn't affect any service-linked role

