# Cisco cloud corner on AWS Lambda
AWS Lambda lets you run code without provisioning or managing servers

## Serverless Frameworks
- [Serverless](https://serverless.com)
- [Claudia.js](https://claudiajs.com)
- [Apex](apex.run)

## Serverless Meetups
- [Paris Serverless Architecture Meetup](https://www.meetup.com/Paris-Serverless-Architecture-Meetup)

## Create a lambda function
1. From the AWS console, go to **Compute** -> **Lambda**
1. Click **Get Started Now** if this is the first time you create a Lambda function or **Create a Lambda function**
1. Select **microservice-http-endpoint** blueprint
1. Enter:
    - Security: **Open** (I know! It's bad)
1. Click **Next**
1. Enter:
    - In Section Configure function:
    - Name: **LambdaNodeJsToDynamoDB**
    - In Section Lambda function handler and role:
    - Role Name: **AccessToDynamoDB**
1. Click **Next**
1. Click **Create function**

## Read an item into DynamoDB using API Gateway and your lambda function
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
    - UserID: **1**
1. Click on + -> Append -> String. Enter:
    - LastName: **Enter your last name**
1. Click **Save**
1. Retry the API Gateway URL

The following message is displayed:
```
{"Items":[{"UserID":"1","LastName":"ARQUES"}],"Count":1,"ScannedCount":1}
```

Congratulations : We have created your first  API Gateway end point and your first lambda function that interacts with DynamoDB

## Insert an item into DynamoDB using API Gateway and your lambda function
1. Install postman add-on
1. Copy/Paste the API Gateway URL of your Lambda function into postman without query parameter
1. Select the **POST** HTTP method
1. Copy/Paste into the body of the POST request:
  ```
  {
      "Item": {
       "UserID": 10,
       "LastName": "CLOUD GUY"
      },
      "TableName": "CloudCorner"
  }  
  ```
1. Send the Request
1. From the AWS console, go to **Database** -> **DynamoDB**
1. Select **CloudCorner** table and check that your item has been added
1. You can also perform the GET request into postman or your web browser by adding query parameter **TableName** with value **CloudCorner**

## Cleanup
1. Delete the **CloudCorner** DynamoDB table
1. Delete the **LambdaMicroservice** API Gateway
1. Delete the **LambdaNodeJsToDynamoDB** Lambda function
1. Delete the **AccessToDynamoDB** IAM Role
1. Delete the **/aws/lambda/LambdaNodeJsToDynamoDB** CloudWatch log group
