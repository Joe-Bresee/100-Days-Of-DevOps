# Day 2

## Problem Statement
To send out a notification via Email, SMS.. when an event occurs.

## Notes
Today, we are using AWS SNS to manage and deliver the sending of messages to subscribing endpoints.

![image](https://github.com/user-attachments/assets/7db783d4-34af-4339-bb2b-01f7c65481e8)

On Day 1, Creating an alarm in CloudWatch created a full monitoring system to allow an administrator effectively monitor for certain events. Now, I'm learning how to use the more general tool, AWS SNS.

SNS has 3 main components:
* Publisher
* Topic
* Subscriber

### Scenario
Today's scenario is simply setting up various SNS notifications. However, since I primarily used the **AWS** console last time, I want to try the CLI and **Terraform**.

### CLI
* Create Topic:
  ```bash
  $ aws sns create-topic --name "my-demo-sns-topic"
  {
    "TopicArn": "arn:aws:sns:us-west-2:1234556667:my-demo-sns-topic"
  }
'''
  
* Subscribe to a Topic
```bash
$ aws sns subscribe --topic-arn arn:aws:sns:us-west-2:123456667:my-demo-sns-topic --protocol email --notification-endpoint test@gmail.com
{
"SubscriptionArn": "pending confirmation"
```

* Publish to a Topic
```bash
'$ aws sns publish --topic-arn arn:aws:sns:us-west-2:1234567:my-demo-sns-topic --message "hello from sns"
{
"MessageId": "d651b7d5-2d66-58c8-abe4-e30822a3aa3e"
}
```

* To list all the subscriptions
```bash
$ aws sns list-subscriptions
{
"Subscriptions": [
{
"Owner": "1234567889",
"Endpoint": "test@gmail.com",
"Protocol": "email",
"TopicArn": "arn:aws:sns:us-west-2:1234567788:HighCPUUtilization",
"SubscriptionArn": "arn:aws:sns:us-west-2:1234567788:HighCPUUtilization:a28e2be8-40cd-4f8b-83d9-33b2c858749d"
}
```

* Unsubscribe from a Topic
```bash
aws sns unsubscribe --subscription-arn arn:aws:sns:us-west-2:1234567899:my-demo-sns-topic:f28124be-850b-4a2e-8d3e-a3dc4f7cca1a
```

* Delete a topic
```bash
$ aws sns delete-topic --topic-arn arn:aws:sns:us-west-2:1234567788:my-demo-sns-topic
```

* List a topic
```bash
$ aws sns list-topics
{
"Topics": [
{
"TopicArn": "arn:aws:sns:us-west-2:123333345555:mydemosnstopic"
}
]
}
```

### Terraform
* Make sure you correctly configure your aws cli on your machine and create an IAM user with sufficient privileges.

```bash
# main.tf
resource "aws_sns_topic" "alarm" {
  name = "alarms-topic"

  delivery_policy = <<EOF
{
  "http": {
    "defaultHealthyRetryPolicy": {
      "minDelayTarget": 20,
      "maxDelayTarget": 20,
      "numRetries": 3,
      "numMaxDelayRetries": 0,
      "numNoDelayRetries": 0,
      "numMinDelayRetries": 0,
      "backoffFunction": "linear"
    },
    "disableSubscriptionOverrides": false,
    "defaultThrottlePolicy": {
      "maxReceivesPerSecond": 1
    }
  }
}
EOF

  provisioner "local-exec" {
    command = "aws sns subscribe --topic-arn ${self.arn} --protocol email --notification-endpoint ${var.alarms_email}"
  }
}
# variables.tf
variable "alarms_email" {
  default = "<>"
}
# outputs.tf
output "sns_topic" {
  value = "${aws_sns_topic.alarm.arn}"
}
```
