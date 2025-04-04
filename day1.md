# Day 1

## Problem Statement
* Create a CloudWatch alarm that sends an email using SNS notification when CPU Utilization is more than 70%.
* Creating a Status Check Alarm to check System and Instance failure and send an email using SNS notification

## Notes
* I did this using the AWS browser console and cli. The **100-Days-of-Devops** repo also has information on how to perform this using Terraform, as Infrastructure as Code, which I also want to experiment with in the future, but for now, I'm sticking with the console and cli.

## Concept Explanation
**AWS CloudWatch** is a service that monitors your **AWS** resources. this is a vital tool for anyone working with a cloud; if there's a spike in traffic or cloud misconfiguration,
**AWS** expenses have the possibility of shooting through the roof and running through your budget. **Cloudwatch** allows us to know when we hit a certain CPU utilization so we can handle our usage.

### Scenario 1: Create a CloudWatch alarm that sends an email using SNS notification when CPU Utilization is more than 70%.

![image](https://github.com/user-attachments/assets/3f63c7fa-8e04-4526-ab0f-a0af2c7bb4ea)

- Launch a free-tier **EC2** instance
- Go to the **CloudWatch** console [here](https://console.aws.amazon.com/cloudwatch/)
- Create a new alarm
- Select your EC2 instance, and CPU usage
- Fill in your details (>=70% usage)
- Select/Create your alarm topic
- Create Alarm

OR
Enter this into your console: 
```console
aws cloudwatch put-metric-alarm --alarm-name cpu-mon --alarm-description "Alarm when CPU exceeds 70 percent" --metric-name CPUUtilization --namespace AWS/EC2 --statistic Average --period 300 --threshold 70 --comparison-operator GreaterThanThreshold  --dimensions "Name=InstanceId,Value=i-12345678" --evaluation-periods 2 --alarm-actions arn:aws:sns:us-east-1:111122223333:MyTopic --unit Percent
```

- To test the alarm, we can use the cli to turn the alarm on.
- Change the alarm-state from INSUFFICIENT_DATA to OK:
```console
aws cloudwatch set-alarm-state --alarm-name "cpu-monitoring" --state-reason "initializing" --state-value OK
```

- Then from OK to Alarm:
```console
aws cloudwatch set-alarm-state --alarm-name "cpu-monitoring" --state-reason "initializing" --state-value ALARM
```

- Check your email

```console
You are receiving this email because your Amazon CloudWatch Alarm "EC2CPUUtilizationAlarm" in the Canada (Central) region has entered the ALARM state, because "initializing" at "Tuesday 01 April, 2025 19:30:51 UTC".

View this alarm in the AWS Management Console:
<>

Alarm Details:
- Name:                       EC2CPUUtilizationAlarm
- Description:
- State Change:               OK -> ALARM
- Reason for State Change:    initializing
- Timestamp:                  Tuesday 01 April, 2025 19:30:51 UTC
- AWS Account:                <>
- Alarm Arn:                  <>

Threshold:
- The alarm is in the ALARM state when the metric is GreaterThanOrEqualToThreshold 70.0 for at least 1 of the last 1 period(s) of 300 seconds.

Monitored Metric:
- MetricNamespace:                     AWS/EC2
- MetricName:                          CPUUtilization
- Dimensions:                          [InstanceId]
- Period:                              300 seconds
- Statistic:                           Average
- Unit:                                not specified
- TreatMissingData:                    breaching


State Change Actions:
- OK:
- ALARM: <>
- INSUFFICIENT_DATA:


--
If you wish to stop receiving notifications from this topic, please click or visit the link below to unsubscribe:
<>

Please do not reply directly to this email. If you have any questions or comments regarding this email, please contact us at https://aws.amazon.com/support
'''

### Scenario 2: Creating a Status Check Alarm to check System and Instance failure and send an email using SNS notification
- On the **AWS console**, this was simply navigating to your instance, and creating a status check alarm. This will fire to the SNS topic if the CU utilization check failed.

```
