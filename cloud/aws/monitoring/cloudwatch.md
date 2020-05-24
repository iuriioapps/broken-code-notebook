# CloudWatch

AWS CloudWatch is a monitoring service to monitor your AWS resources, as well as the applications that run on your AWS.

CloudWatch can be used on-premise - not restricted to just AWS resources. Just need to download and install the SSM agent and a CloudWatch agent.

* EC2 Host level metrics:

  * CPU
  * Network
  * Disk utilization
  * Status check

* Standard monitoring - 5 minutes
* Detailed monitoring - 1 minute

### **What can I do with CloudWatch**

* **Dashboards** - create dashboards to see what is happening with your AWS account
* **Alarms** - allows you to set alarms that notify you when particular thresholds are hit
* **Events** - helps you to respond to state changes in your AWS resources
* **Logs** - helps you aggregate, monitor and store logs

### CloudWatch metrics storage duration

* You can retrieve data using the GetMetricsStatistics API or by using third-party tools offered by AWS partners
* You can store your data in CloudWatch Logs for as long as you want. By default, CloudWatch Logs will store your data indefinitely. You can change the retention for each Log Group at any time.
* You can retrieve data from any terminated EC2 or ELB instance after its termination.

### Granularity

* It depends on AWS service. Many default metrics for many default services are 1 minute, but it can be 3 or 5 minutes depending on the service.
* For custom metrics, the minimum granularity that you can have is 1 minute.

### Alarms

You can create an alarm to monitor any CloudWatch metric in your account. This can include EC2 CPU utilization, ELB latency or even the charges on your AWS bill. You can set the appropriate threshold in which to trigger the alarms and also set what actions should taken if an alarm state is reached.

### Dashboards

Dashboards are customizable home pages in the CloudWatch console that you can use to monitor your resources in a single view, even those resources that are spread across different regions. You can use dashboards to create customized views of the metrics and alarms for your AWS resources.  
To add a widget, change to the region that you need, then add the widget to the dashboard. Remember to save.



