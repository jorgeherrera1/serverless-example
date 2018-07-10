# serverless-example

If you already have an existing Express application, it's very easy to convert to a Serverless-friendly application. Do the following steps:

Install the serverless-http package -- npm install --save serverless-http

Add the serverless-http configuration to your Express application.

You’ll need to import the serverless-http library at the top of your file:

const serverless = require('serverless-http');

then export your wrapped application:

module.exports.handler = serverless(app);.

For reference, an example application might look like this:

# app.js

const serverless = require('serverless-http'); <-- Add this.
const express = require('express')
const app = express()


... All your Express code ...


module.exports.handler = serverless(app); <-- Add this.
Set up your serverless.yml with a single function that captures all traffic:

# serverless.yml

service: express-app

provider:
  name: aws
  runtime: nodejs6.10
  stage: dev
  region: us-east-1

functions:
  app:
    handler: app.handler
    events:
      - http: ANY /
      - http: 'ANY {proxy+}'
That's it! Run sls deploy and your app will deploy!
