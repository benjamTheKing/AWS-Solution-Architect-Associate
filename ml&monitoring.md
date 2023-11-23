Rekognition
------------------
find object people text in images and videos
content moderation

Transcribe
------------------
convert speech to text
remove identity PII
access to multi lingual 

Polly 
------------------
Turn text in speech
pronunciation lexicons
more customization on voice

Translate
------------------

Lex 
------------------
speech into text
chatbot
callcenter bot

Connect
------------------
virtual contact center

Comprehend
------------------
NLP,
group by topic to understand a text 

SageMaker
------------------
ml to dev

Forecast
------------------
Forecast ML

Kendra
------------------
document search service

Personalize
------------------
recommanding products  used by Amazon.com

Textract
------------------
extract text


CloudWatch
------------------
cloudWatch metrics
timestamps
can create cloudwatch dashboard
can create custom metric
can stream metric with 
    - kinesis data firehose -> S3 + AThena to analyse or redshift or opensearch 
    - 3rd party (splunk)

logs store

define logs group 
and you have log stream
log expiration policy
send logs to s3
or streams to other service

cloudwatch logs insights to query logs

cloudwatch logs subscriptions to send data at real time

cloudwatch logs aggregation multiple account multiple region into kinesis data streams 

Cloud watch agent(old), Unified agent(new)
------------------
no logs for ec2, you have to install agent and have iam role, on premise ok
memory usage is a custom metrics so used unified agent to create it

CloudWatch alarms
------------------
3 states
- ok
- insufficient data
- alarm

target: on ec2, asg, sns

cloudwatch composite alarms to trigger on multiple metric
cloudwatch can trigger ec2 instance recovery


EventBridge cloudwatch events
------------------
cron jobs in cloud
replay archived events
archived events

default bus
custom bus
partner bus

event bridge archive and replay if the event need to be stored and used later on 


Cloud Watch container insights, lambda insights, contributors(top 10 ip), application
------------------
if you want for example the 10 hot ip, use cloud watch contributors insight

CloudTrail
------------------
global service
90 days by default
governance, compliance, audit
cloud trail enable by default
event and API

from cloudTrail -> s3(athena to analyse) or cloudwatch logs
investigate cloudTrail first 

cloudtrail insights pay for it, enable it, analyse to detect unusual event

event bridge coupled with CloudTrail

Config
------------------
compliance
record configuration
is there an unrestricted ssh access ? 
is there an alb configuration ? 

config per region
config rules evaluate if ec2 is t2.micro

not change something but overview and compliance

can trigger event bridge