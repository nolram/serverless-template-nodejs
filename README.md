# Serverless Template - Node.js with DynamoDB

A sample Lambda application written in TypeScript and Node.js, using the [Serverless](https://www.serverless.com/) framework. It creates an AWS HTTP API Gateway and a DynamoDB table, and deploys two AWS Lambda functions: `getUser` and `insertUser`.

## Execution
### Deployment
[![asciicast](https://asciinema.org/a/424433.svg)](https://asciinema.org/a/424433)

## Requirements
```
Node 16
Serverless
```

## Running Locally
You need to have a DynamoDB running locally. There's a Python script (requires the `boto` library) and a `docker-compose.yml` file in the [dynamodb-local](dynamodb-local) folder with the necessary local setup. 

Install Serverless
```
npm install -g serverless
```

With DynamoDB running locally and dependencies installed using `yarn`, simply run the command:
```bash
yarn start:dev
```

## Deployment
You need to have AWS credentials in the `~/.aws/credentials` folder with the necessary [permissions](https://www.serverless.com/framework/docs/providers/aws/guide/credentials/) for use.

```bash
sls deploy --stage dev -r us-east-1 -c serverless.yml
```

## Testing
```bash

curl --location --request POST 'https://CHANGE_URL/user' \
--header 'Content-Type: application/json' \
--data-raw '{
    "name": "Marlon",
    "email": "marlonbquadros@gmail.com"
}'

curl --location --request GET 'https://CHANGE_URL/user/user?email=marlonbquadros@gmail.com'
```

## CI/CD Pipeline
In the [.cicd](.cicd) folder, you'll find a [CloudFormation](https://aws.amazon.com/cloudformation/) template that sets up an [AWS CodePipeline](https://aws.amazon.com/codepipeline/) with [AWS CodeBuild](https://aws.amazon.com/codebuild/) for deploying the Lambda service. See the [README.md](.cicd/README.md) in that folder for deployment instructions.

## Directory Structure
```
.
├── .cicd
│   ├── Pipeline.yml
│   ├── README.md
│   └── buildspec
│       └── buildspec_cicd.yml
├── .env-example
├── .eslintignore
├── .eslintrc.js
├── .husky
│   ├── .gitignore
│   ├── pre-commit
│   └── pre-push
├── .nvmrc
├── .prettierignore
├── .prettierrc.js
├── README.md
├── dbconfig.ts
├── dynamodb-local
│   ├── config_dynamodb_local.py
│   ├── docker-compose.yml
│   └── requirements.txt
├── package-lock.json
├── package.json
├── resource
│   └── dynamodb
│       └── DynamoDBTable.yml
├── serverless.yml
├── src
│   ├── app.ts
│   ├── core
│   │   ├── container.ts
│   │   └── services
│   │       └── user.ts
│   ├── env.ts
│   ├── http
│   │   ├── controllers
│   │   │   └── user.ts
│   │   ├── index.ts
│   │   └── schemas
│   │       └── user.ts
│   ├── infrastructure
│   │   ├── adapter
│   │   │   └── dynamodb.ts
│   │   ├── container.ts
│   │   ├── database.ts
│   │   └── repository
│   │       └── user.ts
│   ├── middleware
│   │   └── validator.ts
│   ├── server.ts
│   └── types
│       ├── core.d.ts
│       ├── env.d.ts
│       ├── infrastructure.d.ts
│       └── user.d.ts
└── tsconfig.json
```