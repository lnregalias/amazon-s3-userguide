--------

Welcome to the new **Amazon S3 User Guide**\! The Amazon S3 User Guide combines information and instructions from the three retired guides: *Amazon S3 Developer Guide*, *Amazon S3 Console User Guide*, and *Amazon S3 Getting Started Guide*\.

--------

# Logging options for Amazon S3<a name="logging-with-S3"></a>

You can record the actions that are taken by users, roles, or AWS services on Amazon S3 resources and maintain log records for auditing and compliance purposes\. To do this, you can use server access logging, AWS CloudTrail logging, or a combination of both\. We recommend that you use AWS CloudTrail for logging bucket and object\-level actions for your Amazon S3 resources\. For more information about each option, see the following sections:
+ [Logging requests using server access logging](ServerLogs.md)
+ [Logging Amazon S3 API calls using AWS CloudTrail](cloudtrail-logging.md)

The following table lists the key properties of AWS CloudTrail logs and Amazon S3 server access logs\. Review the table and notes to ensure that AWS CloudTrail meets your security requirements\.


| Log properties | AWS CloudTrail | Amazon S3 server logs | 
| --- |--- |--- |
|  Can be forwarded to other systems \(CloudWatch Logs, CloudWatch Events\)  |  Yes  |  | 
|  Deliver logs to more than one destination \(for example, send the same logs to two different buckets\)  |  Yes  |  | 
|  Turn on logs for a subset of objects \(prefix\)  |  Yes  |  | 
|  Cross\-account log delivery \(target and source bucket owned by different accounts\)  |  Yes  |  | 
|  Integrity validation of log file using digital signature/hashing  |  Yes  |  | 
|  Default/choice of encryption for log files  |  Yes  |  | 
|  Object operations \(using Amazon S3 APIs\)  |  Yes  |  Yes  | 
|  Bucket operations \(using Amazon S3 APIs\)  |  Yes  |  Yes  | 
|  Searchable UI for logs  |  Yes  |  | 
|  Fields for Object Lock parameters, Amazon S3 Select properties for log records  |  Yes  |  | 
|  Fields for `Object Size`, `Total Time`, `Turn-Around Time`, and `HTTP Referer` for log records  |  |  Yes  | 
|  Lifecycle transitions, expirations, restores  |  |  Yes  | 
|  Logging of keys in a batch delete operation  |  |  Yes  | 
|  Authentication failures1  |  |  Yes  | 
|  Accounts where logs get delivered  |  Bucket owner2, and requester  |  Bucket owner only  | 
| Performance and Cost | AWS CloudTrail | Amazon S3 Server Logs | 
| --- |--- |--- |
|  Price  |  Management events \(first delivery\) are free; data events incur a fee, in addition to storage of logs  |  No additional cost in addition to storage of logs  | 
|  Speed of log delivery  |  Data events every 5 mins; management events every 15 mins  |  Within a few hours  | 
|  Log format  |  JSON  |  Log file with space\-separated, newline\-delimited records  | 

**Notes:**

1. CloudTrail does not deliver logs for requests that fail authentication \(in which the provided credentials are not valid\)\. However, it does include logs for requests in which authorization fails \(`AccessDenied`\) and requests that are made by anonymous users\.

1. The S3 bucket owner receives CloudTrail logs only if the account also owns or has full access to the object in the request\. For more information, see [Object\-level actions in cross\-account scenarios](cloudtrail-logging-s3-info.md#cloudtrail-object-level-crossaccount)\. 