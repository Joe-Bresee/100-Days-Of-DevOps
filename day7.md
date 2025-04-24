# AWS S3 Event

## Problem Statement
* Whenever someone deletes an object in an S3 bucket, you will receive a notification

* The **S3 Event** service allows you to recieve notifications based on S3 events. This is useful to monitor your S3 activity for security, logging, and more.

![image](https://github.com/user-attachments/assets/3273ee6a-6bd1-4cfe-80e2-9a791f1bdd03)

![image](https://github.com/user-attachments/assets/6aaee6fb-0c4b-49cd-b6cc-c5a5a59e444f)

## Configuring S3 Events
* Create a new topic (Don't forget to create and verify a subscription to the topic)
* Create a new bucket
* Go to your bucket -> properties -> event notification
* Select your bucket events you want to track (all)
* Choose destination -> SNS-Topic
* Select your s3 topic you made

### Note
After linking my SNS topic, I recieved an error with a not useful at all error description. I decided to use the Amazon Q (Amazon's AI) functionality to help my diagnose the problem, and it said to update the JSON in the SNS topic. This was pretty useful.

### Test it out
* Upload something to your bucket and see if you receive the notification you are expecting.

## Results
```console
{"Records":[{"eventVersion":"2.1","eventSource":"aws:s3","awsRegion":"us-west-2","eventTime":"2025-04-24T20:46:08.525Z","eventName":"ObjectCreated:Put","userIdentity":{"principalId":"redacted"},"requestParameters":{"sourceIPAddress":"redacted"},"responseElements":{"x-amz-request-id":"redacted","x-amz-id-2":"redacted+es3"},"s3":{"s3SchemaVersion":"1.0","configurationId":"s3eventbucketjoeevent","bucket":{"name":"s3eventbucketjoe","ownerIdentity":{"principalId":"redacted"},"arn":"arn:aws:s3:::s3eventbucketjoe"},"object":{"key":"redacted.pdf","size":246949,"eTag":"redacted","sequencer":"redacted"}}}]}

--
If you wish to stop receiving notifications from this topic, please click or visit the link below to unsubscribe:
https://sns.us-west-2.amazonaws.com/unsubscribe.html?SubscriptionArn=redacted

Please do not reply directly to this email. If you have any questions or comments regarding this email, please contact us at https://aws.amazon.com/support
```
