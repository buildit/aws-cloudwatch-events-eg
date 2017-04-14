AWS Management Scripts and Templates
====================================
This project contains assets used to create resources on AWS to help with
cost containment, security, etc.

There are two CloudFormation templates.  Each does a similar thing, but with different
AWS services and results.  Basically, they detect and report "large EC2 instance" creations. 
 
Note that these templates can require the existence of Lambda functions that are managed in a different
project, and which will require their own Cfn stacks
 
CloudTrail Logs Alerts
----------------------
This approach is based upon CloudWatch Log metrics/alerts, SNS, and Lambda.
Since it relies upon CloudTrail and CloudWatch Logs, which is set up in our account to gather all activity in one
trail (Ireland), the Cfn stacks for this approach only have to run in that one region, which is convenient.  The sizable 
disadvantage of this approach is that it is based on CloudWatch Log metrics, which don't provide any detail on 
what particular event incremented the metric.  Thus, notifications are pretty minimal:  "A large instance was created".


CloudWatch Events Alerts
------------------------
This approach is based upon CloudWatch Events and Lambda.  One or many CloudWatch Events rules are created, which
result in Lambda function executions.  This approach is conceptually very simple and direct compared to the CloudTrail
Logs approach, but requires the Cfn stacks to be defined in each region.

Note also that for simplicity's sake, we're targeting direct invocations of a Lambda functions.  It would probably be
more robust to instead target SNS topics and have Lambdas (and whatever else) subscribe to those topics (similar to
the CloudTrail Logs approach, above).  However, in the interest of time-boxing we just went with direct invocation.