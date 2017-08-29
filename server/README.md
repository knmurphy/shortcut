# tal-server
A node/express server that
- serves a single page application ("tal-client")
- provides an API for handling social authentication for the client. Currently Twitter and Facebook are supported.
- provides web hooks for updating individual episodes and the list of available episodes
- ensures that only authorized requests can access episode data

## Development

### Pre-reqs

##### Install dependencies
```
npm install
npm install -g nodemon
```

##### Env Variables
Copy `.env-template` to `.env` and fill in environment variables. 

A couple environment variables to note:
- `AWS_KMS_KEY_ID` - oAuth tokens are encrypted with AWS KMS (Key Management Service). Create this from the AWS IAM console, add the key ID to the .env file (AWS_KMS_KEY_ID), and make sure the AWS IAM entity has access in the AWS region. In "tal-serverless", ensure that the IAM role that executes the social share function also has access to the key.

- `APP_VERSION` - version of the tal-client JS bundle, i.e. `//d2wyy28kxzazzz.cloudfront.net/app_${APP_VERSION}.js`. See "tal-client" for info on how to build this.

*NOTE: Environment Variables are not automatically copied over to Elastic Beanstalk deployments. Enter the environment variables manually in the EB console.

### Run Locally

### Build & run with Docker:
- Mac only: eval "$(docker-machine env default)"
- `docker build -t tal-server .`
- `docker run -p 3000:3000 -d tal-server`
- server will be running at docker-machine ip, i.e. `http://192.168.99.100:3000/`

### Run with Node:
- `npm start`
- `npm run dev` # run with nodemon
Runs on `localhost:3000`

## Deploy

### Deploy via AWS Elastic Beanstalk
- [AWS notes on deploying node.js to ebs](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/create_deploy_nodejs.html)
- Or, [use the CLI](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb-cli3-configuration.html):
	- `eb deploy`
	- **Deploy Scripts** (these assume you have IAM credentials for `shortcut` in `~/.aws/credentials`)
		- `bash deploy.sh` --> deploy to production
		- `bash deploy-staging.sh` --> deploy to staging
- set environment variables from .env file by running `bash setenv.sh`, or via AWS website: All Applications --> <app> --> <app environment> --> configuration --> Environment Properties
- Tested with Node.js v 4.4.3