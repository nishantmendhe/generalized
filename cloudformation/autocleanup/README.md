# LambdaSchool

This repository contains the template for the Lambda School project. This template is in charge of stopping EC2, RDS and SageMaker instaces when the estimated budget is reached.

## LambdaSchool Flowchart

<p align="center">
  <img src="img/LambdaSchool.png"/>
</p>

### How it works?

The *AWS Budget* resource it's created to stablish a maximum amount of money that can be spent. When the cost of the resources exceed the 100% of the stablished budget, it would activate an *AWS SNS (Simple Notification Service)* to create a *Topic* and publish notifications in all the subscribers, in this case a *Lambda Function*. 


### Elements

* AWS Budgets: budget definition for the student, this will trigger an SNS Message to start the cleaning process

* Amazon SNS: SNS element that will publish a message to start the execution of the cleaning proccess

* State Machine: Lambda in charge of executing the process in the state machine (Step Functions)

* State Machine: Lambda functions orchestration to start the process of stopping EC2, RDS and Sage Maker instaces.

* SNS and Email Notification: notification to the user about the results of the process.

### State Machine

States Diagram:

<p align="center">
  <img src="img/StateMachine.png"/>
</p>

* Params: pass type state to initialize parameters for the state machine executions

* Clean: Lambda function in charge of stopping instances and checking their status

* Decision: choice type state in charge of deciding if the process should wait for instaces that are still running

* SnsNotification: Lambda function that sends information to user email about the result of the process


## Infrastructure

This template was built using AWS Cloudformation Nested Stacks. The *master* files is in charge to create all the resources needed.

### Parameters

<center>

| Parameter        | Description           | Type    |
| ------------- |:-------------:| -----:|
| S3Bucket      | S3 Bucket where the template is hosted | String |
| WaitingTime | Time in minutes that will wait until retrying again (1 up to 15 minutes)      |    Number |
| EmailAddress | Email address to send notification when the cleaning process ends     |    Number |
| RetryTimes | The number of times the process will try to clean the resources if they are still running  |    Number |
| BudgetAmout | Budget amount that will trigger the clean up process, if the billing goes higher than the amount    |    Number |


</center>