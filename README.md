# Cisco cloud corner on AWS Lambda
AWS Lambda lets you run code without provisioning or managing servers

## Serverless Frameworks:
- [Serverless](https://serverless.com)
- [Claudia.js](https://claudiajs.com)
- [Apex](apex.run)

## Serverless Meetups:
- [Paris Serverless Architecture Meetup](https://www.meetup.com/Paris-Serverless-Architecture-Meetup)

## Create a lambda function
1. From the AWS console, go to **Compute** -> **Lambda**
1. Click **Create a Lambda function**
1. Select **microservice-http-endpoint** blueprint
1. Enter:
    - Security: **Open**
1. Click **Next**
1. Enter:
    - Section **Configure function**
    - Name: **LambdaNodeJsToDynamoDB**
    - Section **Lambda function handler and role**
    - Role Name: **AccessToDynamoDB**
1. Click **Next**
1. Click **Create function**

## Trigger a lambda function using API Gateway
When the Lambda function has been created
1. Goto **Trigger** tab and click to the API Gateway URL.

  The following message is displayed:
  ```
  {"message": "Internal server error"}
  ```
  This is because the Lambda function is expecting a query parameter named **TableName** followed by the DynamoDB table to read

1. Add the query parameter **?TableName=CloudCorner** at the end of the API Gateway URL

  The following message is displayed:
  ```
  Requested resource not found
  ```
  This is because the DynamoDB table **CloudCorner** doesn't exist

1. From the AWS console, go to **Database** -> **DynamoDB**
1. Select **Create Table**
  - Table name: **CloudCorner**
  - Primary key: **UserID** **Number**
  - Click **Create**

  Wait until the **CloudCorner** table has been created

1. Select the **CloudCorner** table
1. Go to folder **Items** and select **Create Item**
1. Enter:
    - UserId: **1**
1. Click on + -> Append -> String. Enter:
    - LastName: **Enter your last name**
1. Click **Save**
1. Retry the API Gateway URL
The following message is displayed:
```
{"Items":[{"UserID":"1","LastName":"ARQUES"}],"Count":1,"ScannedCount":1}
```
