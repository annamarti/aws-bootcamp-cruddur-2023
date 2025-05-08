# Week 0 — Billing and Architecture and Setup

1. Create conceptual diagram in lucid.app
https://lucid.app/lucidchart/a3336ef5-307d-40d1-b8da-862a6936b0dc/edit?invitationId=inv_3eeb862d-9e44-4271-8097-a4006073fddd

2. Create gihub repo copied from there via 'Use this template':
````
https://github.com/ExamProCo/aws-bootcamp-cruddur-2023
````
3. Create Gitpod account and add gitpod plugin to browser: 
````
https://gitpod.io/
````
4. Create AWS account, IAM user, generate Access Key for IAM user:
````
https://aws.amazon.com/
````

5. Open gihub repo in gitpod, install AWS CLI into .gitpod.yml 
```
tasks:
  - name: aws-cli
    env:
      AWS_CLI_AUTO_PROMPT: auto
    init: |
      cd /workspace
      curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
      unzip awscliv2.zip
      sudo ./aws/install
      cd $THEIA_WORKSPACE_ROOT
```
6. Configure AWS CLI.
ACCESS KEY and SECRET ACCESS KEY from AWS IAM user, should be taken when generated key(in step 4:) 
```
gp env AWS_ACCESS_KEY_ID="AKIAXYKJQ4**********"
gp env AWS_SECRET_ACCESS_KEY="Tuu0JvVdAazKetcKejUFKtJ+AhCfbC**********"
gp env AWS_DEFAULT_REGION="us-east-1"
gp env ACCOUNT_ID=$(aws sts get-caller-identity --query Account --output text)
```

7. Add budget. Add json files for budget, then run cli command:
````
aws budgets create-budget \
    --account-id 533267015091 \
    --budget file://aws/json/budget.json \
    --notifications-with-subscribers file://aws/json/budget-notifications-with-subscribers.json
```` 
This command creates budget after each run, each budget after 2, costs a dollar. Delete budget:
````
aws budgets delete-budget --account-id 533267015091 --budget-name "Example Tag Budget"
````

8. Add sns topic:
````
aws sns create-topic --name biling-alarm
````
This will gave you topic arn, use it to subsrcibe
````
aws sns subscribe \
    --topic-arn arn:aws:sns:us-east-1:533267015091:biling-alarm \
    --protocol email \
    --notification-endpoint anna.martirosyan@mail.com
````

9. Add CloudWatch alarm. Add alarm-config.json file, then run cli:
````
aws cloudwatch put-metric-alarm --cli-input-json file://aws/json/alarm-config.json
````
