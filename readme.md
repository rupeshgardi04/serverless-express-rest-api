# Deploying REST API using Serverless, Express and Node.js

## Getting Started
To get started, you'll need the Serverless Framework installed. 
[Serverless Framwork](https://www.serverless.com/framework/docs/providers/aws/guide/quick-start/) 

You'll also need your environment configured with AWS credentials.
1. _For this, Better to create IAM User by using your root account. Get you access and secrete keys._
2. _use aws configure command_
> aws configure

> aws iam get-user

## Create and deploy a single endpoint
1. create a folder lets say `express-aws`
2. open that folder in terminal and run `npm init -f` command and it will create package.json file.
3. let's install dependencies `express` and `serverless-http`
command: `npm install --save express serverless-http`. 
serverless-http is a middleware that handles the interface between our Node.js application and specific API Gateway.
4. Now we create index.js file with below code
```javascript
const serverless = require('serverless-http');
const express = require('express');
const app = express();

app.get('/', function (req, res) {
    res.send('hello rupesh')    
})

module.exports.handler = serverless(app);
```
this is simple application which return `hello rupesh` when we visit API Gateway endpoint in browser.

Here we have imported the `serverless-http` package at the top. Second, we exported a `handler function` which is our application wrapped in the serverless package.

5. Now create serverless.yml in our working directory:
```javascript
service: rupesh-express-application

provider:
  name: aws
  runtime: nodejs10.x
  stage: dev
  region: us-east-1

functions:
  app:
    handler: index.handler
    events:
      - http: ANY /
      - http: 'ANY {proxy+}'
```
here we have created one function, app, which uses the exported handler from our index.js file.

## Deploy your function:
1. use command `serverless deploy`
we can see output like this
```
$ sls deploy
Serverless: Packaging service...
Serverless: Excluding development dependencies...
Serverless: Creating Stack...
Serverless: Checking Stack create progress...
.....
Serverless: Stack create finished...
Serverless: Uploading CloudFormation file to S3...
Serverless: Uploading artifacts...
Serverless: Uploading service rupesh-express-application.zip file to S3 (744.11 KB)...
Serverless: Validating template...
Serverless: Updating Stack...
Serverless: Checking Stack update progress...
.................................
Serverless: Stack update finished...
Service Information
service: rupesh-express-application
stage: dev
region: us-east-1
stack: rupesh-express-application-dev
resources: 11
api keys:
  None
endpoints:
  ANY - https://b232s46o32.execute-api.us-east-1.amazonaws.com/dev
  ANY - https://b232s46o32.execute-api.us-east-1.amazonaws.com/dev/{proxy+}
functions:
  app: rupesh-express-application-dev-app
layers:
  None

now, visit above endpoint in browser and you will see hello rupesh printed.

Congratulations!! Your application is live!

