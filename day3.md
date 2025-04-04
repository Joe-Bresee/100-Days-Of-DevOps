a# Day 3: Introduction to CloudTrail

## Problem Statement: 
Logs all the API calls in an AWS account(including AWS Console, CLI and API/SDK calls)

## Notes
Today I will be completing this via the **AWS CloudShell** (CLI)
**AWS CloudTrail** is a service that assists administrators in enabling government, compliance, risk auditing.

![image](https://github.com/user-attachments/assets/b43d4b1c-1c02-4402-9701-32eaaac2d14d)

* To create an ongoing record of activity, you create something called a **trail**. A **trail** is a service to send event logs to a **S3 bucket**.
* A trail allows options like regions, event filtering, data, API activity, Lambda invokation, storage location, encryption & SNS notifications. This allows one to make their events easier to track, filter, and manage, since for example some regulations are region dependent (European Union's strict privacy laws, etc.) or some events require immediate attention and action.
* Trails have file integrity validation, meaning details on any changes to the log can be viewed.

### Create a S3 bucket
* Note your s3 bucket shares a public namespace so make it memorable but unique.
* Note you will have to enable CloudTrail on your bucket's policy.
```console
aws s3 mb s3://mytests3bucketforcloudtrail --region <your-region>
```

### Create Trail

```console
$ aws cloudtrail create-trail --name my-test-cloudtrail --s3-bucket-name mytests3bucketforcloudtrail
{
"IncludeGlobalServiceEvents": true,
"IsOrganizationTrail": false,
"Name": "my-test-cloudtrail",
"TrailARN": "arn:aws:cloudtrail:us-west-2:XXXXXXXXX:trail/my-test-cloudtrail",
"LogFileValidationEnabled": false,
"IsMultiRegionTrail": false,
"S3BucketName": "mytests3bucketforcloudtrail"
}
```

* For a multi-regional trail:
```console
$ aws cloudtrail create-trail --name my-test-cloudtrail-multiregion --s3-bucket-name mytests3bucketforcloudtrail --is-multi-region-trail
{
"IncludeGlobalServiceEvents": true,
"IsOrganizationTrail": false,
"Name": "my-test-cloudtrail-multiregion",
"TrailARN": "arn:aws:cloudtrail:us-west-2:XXXXXXXXX:trail/my-test-cloudtrail-multiregion",
"LogFileValidationEnabled": false,
"IsMultiRegionTrail": true,
"S3BucketName": "mytests3bucketforcloudtrail"
}
```

* To get status of all trails
```console
$ aws cloudtrail describe-trails
{
"trailList": [
{
"IncludeGlobalServiceEvents": true,
"IsOrganizationTrail": false,
"Name": "my-test-cloudtrail",
"TrailARN": "arn:aws:cloudtrail:us-west-2:XXXXXXXXXX:trail/my-test-cloudtrail",
"LogFileValidationEnabled": false,
"IsMultiRegionTrail": false,
"HasCustomEventSelectors": false,
"S3BucketName": "mytests3bucketforcloudtrail",
"HomeRegion": "us-west-2"
},
{
"IncludeGlobalServiceEvents": true,
"IsOrganizationTrail": false,
"Name": "my-test-cloudtrail-multiregion",
"TrailARN": "arn:aws:cloudtrail:us-west-2:XXXXXXXXXX:trail/my-test-cloudtrail-multiregion",
"LogFileValidationEnabled": false,
"IsMultiRegionTrail": true,
"HasCustomEventSelectors": false,
"S3BucketName": "mytests3bucketforcloudtrail",
"HomeRegion": "us-west-2"
}
```

* Start logging for trail
```console
$ aws cloudtrail start-logging --name my-test-cloudtrail
```

* Verifying if logging is enabled:
```console
$ aws cloudtrail get-trail-status --name my-test-cloudtrail
{
"LatestDeliveryTime": 1550014519.927,
"LatestDeliveryAttemptTime": "2019-02-12T23:35:19Z",
"LatestNotificationAttemptSucceeded": "",
"LatestDeliveryAttemptSucceeded": "2019-02-12T23:35:19Z",
"IsLogging": true,
"TimeLoggingStarted": "2019-02-12T23:34:58Z",
"StartLoggingTime": 1550014498.331,
"LatestNotificationAttemptTime": "",
"TimeLoggingStopped": ""
}
```

*Enabling Log validation

```console
$ aws cloudtrail create-trail --name my-test-cloudtrail-multiregion-logging --s3-bucket-name mytests3bucketforcloudtrail --is-multi-region-trail --enable-log-file-validation
{
"IncludeGlobalServiceEvents": true,
"IsOrganizationTrail": false,
"Name": "my-test-cloudtrail-multiregion-logging",
"TrailARN": "arn:aws:cloudtrail:us-west-2:349934551430:trail/my-test-cloudtrail-multiregion-logging",
"LogFileValidationEnabled": true,
"IsMultiRegionTrail": true,
"S3BucketName": "mytests3bucketforcloudtrail"
}
```

* Deleting a trail
```console
$ aws cloudtrail delete-trail --name my-test-cloudtrail-multiregion-logging
```
