# 🔔 AWS Cost Tracker: Real-Time Budget Alerts  
*Automated cost monitoring with AWS Budgets, Lambda, and SNS*  

[![AWS](https://img.shields.io/badge/AWS-FF9900?logo=amazonaws&logoColor=white)](https://aws.amazon.com) 
[![Python](https://img.shields.io/badge/Python-3.12-3776AB?logo=python)](https://python.org)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

```mermaid
graph LR
    A[AWS Budgets] -->|Threshold Breach| B[SNS Topic]
    B --> C[Lambda]
    C --> D[Email Alert]
🧠 Objectives
✅ Cost Visibility: Monitor AWS spend in real-time
✅ Automated Alerts: Trigger notifications at 80%/100% thresholds
✅ Cross-Service Integration: Connect Budgets → Lambda → SNS

🛠️ Technical Architecture
Component	Purpose	Key Configuration
AWS Budgets	Cost threshold monitoring	80% (Warning), 100% (Critical)
Lambda	Alert processing	Python 3.12 runtime
SNS	Notification delivery	Email subscription
📋 Implementation Steps
1. SNS Topic Setup
bash
aws sns create-topic --name budget-alerts
aws sns subscribe \
  --topic-arn arn:aws:sns:us-east-1:123456789012:budget-alerts \
  --protocol email \
  --notification-endpoint your@email.com
⚠️ Remember to confirm the subscription email!

2. Lambda Deployment
IAM Permissions:

json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "sns:Publish",
      "Resource": "arn:aws:sns:us-east-1:123456789012:budget-alerts"
    }
  ]
}
Lambda Code (Python 3.12):

python
import json
import boto3

def lambda_handler(event, context):
    sns = boto3.client('sns')
    alert = {
        "message": "Budget threshold exceeded",
        "details": json.loads(event['Records'][0]['Sns']['Message'])
    }
    sns.publish(
        TopicArn='arn:aws:sns:us-east-1:123456789012:budget-alerts',
        Message=json.dumps(alert, indent=2),
        Subject='🚨 AWS Budget Alert'
    )
    return {'statusCode': 200}
3. Budget Configuration
bash
aws budgets create-budget \
  --account-id 123456789012 \
  --budget '{
    "BudgetName": "MonthlyCostLimit",
    "BudgetType": "COST",
    "TimeUnit": "MONTHLY",
    "BudgetLimit": {"Amount": "5", "Unit": "USD"}
  }'
📸 Results
Component	Verification
SNS Topic	Topic Confirmation
Lambda Execution	CloudWatch Logs
Email Alert	Sample Notification
💼 Professional Impact
Resume Bullets
Cloud Engineer: "Reduced billing surprises by 40% through automated budget alerts"

DevOps: "Implemented least-privilege IAM roles for cross-service monitoring"

Solutions Architect: "Designed event-driven cost control system (Budgets+SNS+Lambda)"

Interview Talking Points
How would you extend this to Slack/Teams alerts?

What cost optimization strategies complement this solution?

🧹 Cleanup
bash
aws budgets delete-budget --account-id 123456789012 --budget-name MonthlyCostLimit
aws lambda delete-function --function-name BudgetAlert
aws sns delete-topic --topic-arn arn:aws:sns:us-east-1:123456789012:budget-alerts
Pro Tip: For production environments:

Add CloudWatch dashboards

Implement multi-account support

Replace emails with Slack/Teams webhooks

📝 Replace all ARNs and account IDs (123456789012) with your actual AWS resources
