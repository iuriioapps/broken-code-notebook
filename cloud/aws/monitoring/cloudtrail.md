# CloudTrail



* Enables
  * After-the-fact incident investigation
  * Near realtime intrusion detection
  * Industry and regulatory compliance
* Provides
  * Logs API call details \(for supported services\)
* What is logged
  * Metadata around API call
  * The identity of the API caller
  * The time of the API call
  * The source IP address of the API caller
  * The request parameters
  * The response elements returned by the service
* CloudTrail event logs:
  * Sent to an S3 bucket
  * You manage the retention in S3
  * Delivered every 5 minutes with up 15 minutes delay
  * Notification available
  * Can be aggregated across regions
  * Can be aggregated across accounts
* Validating CloudTrail log file integrity:
  * Was the log files modified or deleted?
  * CloudTrail log file integrity validation
    * SHA-256 hashing
    * SHA-256 hashing with RSA for digital signing
  * Log files are delivered with a 'digest' file \(if enabled\)
  * Digest file can be used to validate the integrity of the log files
* Securing CloudTrail logs
  * Use IAM policies and S3 bucket policies to restrict access to the S3 bucket containing the log files. Place employees who have a security role, into IAM group with attached policies that enable access to the logs.
  * Use SSE-S3 or SSE-KMS to encrypt the logs
  * Configure SNS notifications and log file validation on the 'Trail'. Develop a solution that when triggered by SNS will validate the logs using the provided digest file.
  * Restrict delete access with IAM and bucket policies. Configure S3 MFA Delete. Validate that logs have not been deleted using the log file validation.
  * By default logs will be kept indefinitely. Use S3 object lifecycle management to remove the files after the required period of time, or move the files to AWS Glacier for more cost-effective long term storage.

