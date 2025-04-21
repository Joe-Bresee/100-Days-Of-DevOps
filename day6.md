#Day 6: Cloudwatch Logs

## Problem: I want to deploy a simple monitoring system when any unauthorized person is trying to access my servers, I will recieve a notification via SNS.

## Solution
Cloudwatch Metric Filters with SNS notifications

![image](https://github.com/user-attachments/assets/4f53e975-8526-4fe1-ab09-9923699b5363)

* Ensure CloudWatch Agent is installed.
* Go to CloudWatch Logs and search for invalid user string
* Add a metric filter for invalid users
* Create the alarm
* Select threshold and SNS topic
