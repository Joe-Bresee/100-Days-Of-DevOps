# Day 4: CloudWatch Log Agent Installation

## Problem Statement
* Collect the system logs & push to **CloudWatch Logs**
* Collect custom metrics

## Notes
* I will do this today via the **AWS Console**

## Solution
* **CloudWatch Logs** are used to collect information from **EC2** instances and other sources.
* Having log information in one centralized place allows you to easily monitor and view your systems to ensure security, efficiency, and scalability. It helps engineers see inside a system and determine the best steps to keep the system functioning optimally.


![image](https://github.com/user-attachments/assets/6301858e-381f-47ef-9514-8ddf7cc57ae7)

### Scenario 1: Perform manually
* Collect system and application logs, as well as metrics.

* Set up IAM role for **AWS Service** **EC2**
* Check 2 policies: **AmazonSSMFullAccess**, **CloudWatchAgentServerPolicy**
* Create role
* Attach role to an EC2 instance
* ssh intot your EC2
* Install CloudWatch Agent
```console
# wget https://s3.amazonaws.com/amazoncloudwatch-agent/linux/amd64/latest/AmazonCloudWatchAgent.zip
# unzip AmazonCloudWatchAgent.zip
# ./install.sh
```
*If ./intstall.sh doesn't work try sudo

* Follow the setup wizard instructions. Due to the guide being slightly outdated, some additional options are there - Just select defaults.
```console
# Invoke the installer(This will step us through, how to install the agent)
# sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-config-wizard
=============================================================
= Welcome to the AWS CloudWatch Agent Configuration Manager =
=============================================================
On which OS are you planning to use the agent?
1. linux
2. windows
default choice: [1]:
Trying to fetch the default region based on ec2 metadata...
Are you using EC2 or On-Premises hosts?
1. EC2
2. On-Premises
default choice: [1]:
Do you want to turn on StatsD daemon?
1. yes
2. no
default choice: [1]:
Which port do you want StatsD daemon to listen to?
default choice: [8125]
What is the collect interval for StatsD daemon?
1. 10s
2. 30s
3. 60s
default choice: [1]:
What is the aggregation interval for metrics collected by StatsD daemon?
1. Do not aggregate
2. 10s
3. 30s
4. 60s
default choice: [4]:
Do you want to monitor metrics from CollectD?
1. yes
2. no
default choice: [1]:
Do you want to monitor any host metrics? e.g. CPU, memory, etc.
1. yes
2. no
default choice: [1]:
Do you want to monitor cpu metrics per core? Additional CloudWatch charges may apply.
1. yes
2. no
default choice: [1]:
2
Do you want to add ec2 dimensions (ImageId, InstanceId, InstanceType, AutoScalingGroupName) into all of your metrics if the info is available?
1. yes
2. no
default choice: [1]:
Would you like to collect your metrics at high resolution (sub-minute resolution)? This enables sub-minute resolution for all metrics, but you can customize for specific metrics in the output json file.
1. 1s
2. 10s
3. 30s
4. 60s
default choice: [4]:
Which default metrics config do you want?
1. Basic
2. Standard
3. Advanced  <--Make sure you choose advance then only you will see(Memory,Swap,Disk...)
4. None
default choice: [1]:
3
Current config as follows:
{
"metrics": {
"append_dimensions": {
"AutoScalingGroupName": "${aws:AutoScalingGroupName}",
"ImageId": "${aws:ImageId}",
"InstanceId": "${aws:InstanceId}",
"InstanceType": "${aws:InstanceType}"
},
"metrics_collected": {
"collectd": {
"metrics_aggregation_interval": 60
},
"cpu": {
"measurement": [
"cpu_usage_idle",
"cpu_usage_iowait",
"cpu_usage_user",
"cpu_usage_system"
],
"metrics_collection_interval": 60,
"totalcpu": false
},
"disk": {
"measurement": [
"used_percent",
"inodes_free"
],
"metrics_collection_interval": 60,
"resources": [
"*"
]
},
"diskio": {
"measurement": [
"io_time",
"write_bytes",
"read_bytes",
"writes",
"reads"
],
"metrics_collection_interval": 60,
"resources": [
"*"
]
},
"mem": {
"measurement": [
"mem_used_percent"
],
"metrics_collection_interval": 60
},
"netstat": {
"measurement": [
"tcp_established",
"tcp_time_wait"
],
"metrics_collection_interval": 60
},
"statsd": {
"metrics_aggregation_interval": 60,
"metrics_collection_interval": 10,
"service_address": ":8125"
},
"swap": {
"measurement": [
"swap_used_percent"
],
"metrics_collection_interval": 60
}
}
}
}
Are you satisfied with the above config? Note: it can be manually customized after the wizard completes to add additional items.
1. yes
2. no
default choice: [1]:
Do you have any existing CloudWatch Log Agent (http://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/AgentReference.html) configuration file to import for migration?
1. yes
2. no
default choice: [2]:
Do you want to monitor any log files?
1. yes
2. no
default choice: [1]:
Log file path:
/var/log/messages
Log group name:
default choice: [messages]
Log stream name:
default choice: [{instance_id}]
Do you want to specify any additional log files to monitor?
1. yes
2. no
default choice: [1]:
Log file path:
/var/log/secure
Log group name:
default choice: [secure]
Log stream name:
default choice: [{instance_id}]
Do you want to specify any additional log files to monitor?
1. yes
2. no
default choice: [1]:
2
Saved config file to /opt/aws/amazon-cloudwatch-agent/bin/config.json successfully.
Current config as follows:
{
"logs": {
"logs_collected": {
"files": {
"collect_list": [
{
"file_path": "/var/log/messages",
"log_group_name": "messages",
"log_stream_name": "{instance_id}"
},
{
"file_path": "/var/log/secure",
"log_group_name": "secure",
"log_stream_name": "{instance_id}"
}
]
}
}
},
"metrics": {
"append_dimensions": {
"AutoScalingGroupName": "${aws:AutoScalingGroupName}",
"ImageId": "${aws:ImageId}",
"InstanceId": "${aws:InstanceId}",
"InstanceType": "${aws:InstanceType}"
},
"metrics_collected": {
"collectd": {
"metrics_aggregation_interval": 60
},
"cpu": {
"measurement": [
"cpu_usage_idle",
"cpu_usage_iowait",
"cpu_usage_user",
"cpu_usage_system"
],
"metrics_collection_interval": 60,
"totalcpu": false
},
"disk": {
"measurement": [
"used_percent",
"inodes_free"
],
"metrics_collection_interval": 60,
"resources": [
"*"
]
},
"diskio": {
"measurement": [
"io_time",
"write_bytes",
"read_bytes",
"writes",
"reads"
],
"metrics_collection_interval": 60,
"resources": [
"*"
]
},
"mem": {
"measurement": [
"mem_used_percent"
],
"metrics_collection_interval": 60
},
"netstat": {
"measurement": [
"tcp_established",
"tcp_time_wait"
],
"metrics_collection_interval": 60
},
"statsd": {
"metrics_aggregation_interval": 60,
"metrics_collection_interval": 10,
"service_address": ":8125"
},
"swap": {
"measurement": [
"swap_used_percent"
],
"metrics_collection_interval": 60
}
}
}
}
Please check the above content of the config.
The config file is also located at /opt/aws/amazon-cloudwatch-agent/bin/config.json.
Edit it manually if needed.
Do you want to store the config in the SSM parameter store?
1. yes
2. no
default choice: [1]:
1
What parameter store name do you want to use to store your config? (Use 'AmazonCloudWatch-' prefix if you use our managed AWS policy)
default choice: [AmazonCloudWatch-linux]
CloudWatch-Advanceconfig-linux
Trying to fetch the default region based on ec2 metadata...
Which region do you want to store the config in the parameter store?
default choice: [us-west-2]
Which AWS credential should be used to send json config to parameter store?
1. XXXXXXXX(From SDK) <-- As we are using Roles
2. Other
default choice: [1]:
Successfully put config to parameter store CloudWatch-Advanceconfig-linux.
Program exits now.
```

* Validate your setup
```console
 sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch-config -m ec2 -c file:/opt/aws/amazon-cloudwatch-agent/bin/config.json -s
```

*I had to install collectd for it to work:
```console
sudo yum install collectd
```


### Scenario 2: Using SSM Parameter Store
```console
# wget https://s3.amazonaws.com/amazoncloudwatch-agent/linux/amd64/latest/AmazonCloudWatchAgent.zip
# unzip AmazonCloudWatchAgent.zip
# ./install.sh
```

* Instead of using the wizard to setup, we can use sys manager parameter store

```console
# sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch-config -m ec2 -c ssm:CloudWatch-Advanceconfig-linux -s
/opt/aws/amazon-cloudwatch-agent/bin/config-downloader --output-dir /opt/aws/amazon-cloudwatch-agent/etc/amazon-cloudwatch-agent.d --download-source ssm:CentosCloudWatch-linux --mode ec2 --config /opt/aws/amazon-cloudwatch-agent/etc/common-config.toml --multi-config default
Region: us-west-2
credsConfig: map[]
Successfully fetched the config and saved in /opt/aws/amazon-cloudwatch-agent/etc/amazon-cloudwatch-agent.d/ssm_CentosCloudWatch-linux.tmp
Start configuration validation...
/opt/aws/amazon-cloudwatch-agent/bin/config-translator --input /opt/aws/amazon-cloudwatch-agent/etc/amazon-cloudwatch-agent.json --input-dir /opt/aws/amazon-cloudwatch-agent/etc/amazon-cloudwatch-agent.d --output /opt/aws/amazon-cloudwatch-agent/etc/amazon-cloudwatch-agent.toml --mode ec2 --config /opt/aws/amazon-cloudwatch-agent/etc/common-config.toml --multi-config default
Valid Json input schema.
No csm configuration found.
Configuration validation first phase succeeded
/opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent -schematest -config /opt/aws/amazon-cloudwatch-agent/etc/amazon-cloudwatch-agent.toml
Configuration validation second phase succeeded
Configuration validation succeeded
Created symlink from /etc/systemd/system/multi-user.target.wants/amazon-cloudwatch-agent.service to /etc/systemd/system/amazon-cloudwatch-agent.service.
Redirecting to /bin/systemctl restart amazon-cloudwatch-agent.service
```

* If everything works correctly, go to your CloudWatch dashboard on the console and you can see your logs. Now you have visibility into your EC2 instances. Note this also works for on-prem, which is probably useful for those who have a mixed environment in AWS and on-prem.
