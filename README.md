# How to protect a self-managed DNS service against DDoS attacks using AWS Global Accelerator and AWS Shield Advanced - Blog Post

This repository holds supporting documentation for the AWS Security Blog post, How to protect a self-managed DNS service against DDoS attacks using AWS Global Accelerator and AWS Shield Advanced. This helps customers implement a solution to protect their existing self managed DNS servers using [AWS Shield Advanced](http://aws.amazon.com/shield) and [AWS Global Accelerator](http://aws.amazon.com/global-accelerator)

The AWS CloudFormation templates do not create the resources to be protected, e.g. the AWS Elastic Compute Cloud (EC2) instances the DNS is running on. It is assumed this infrastructure already exists.

## CloudFormation Templete Overview


dns_health_checker.yml - Creates a single CloudFormation stack in the region of your choice that deploys the following resources:
- DnsResolverLambda : Lambda function that sends a DNS query to the IP addresses that you specify which should match your AWS Global Accelerator accelerator’s Global IP addresses. The function then posts a 0 or 1 value to CloudWatch metrics depending on the success or failure of DNS server to respond to the query.
- LambdaExecutionRole : IAM role used by DnsResolverLambda during execution.
- LambdaEventsRule : Events rule that invokes DnsResolverLambda every minute.
- CloudWatchDnsHcAlarm : CloudWatch Alarm that tracks the CloudWatch metric generated by DnsResolverLambda and goes into an ALARM state if the metric value is greater than 0.
- Route53HealthCheck [Optional]: Route 53 health check that tracks the the state of the CloudWatchDnsHcAlarm. You can associate this health check with your AWS Shield Advanced protected resource.

## Security

See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.

## License

This library is licensed under the MIT-0 License. See the LICENSE file.

