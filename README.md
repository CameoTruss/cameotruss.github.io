Cameo Shift Monitor

Death to filing cabinets 

# Overview

## Shift Monitor App

The Shift Monitor web app is used by production staff on the factory floor to time how long each truss takes to assemble. It is accessed via Android tablets which are connected to semi-reliable wifi and is hosted on AWS. 

## Business Value

### Predictive Model of Assembly Time

Labour and factory utilization are significant factors in the cost to Cameo to produce trusses. Moreover there is a lot of variability in the size and shape of trusses (ie their "difficulty"). This leads to an unpredictable amount of resources being spent to assemble trusses and ultimately to fulfill orders. 

**Since the revenue (ie quoted price) generated from an order is set before assembly begins (ie at design time) it is valuable to be able to accurately predict the cost to fulfil an order from information available at design time. **

There are numerous extractable quantitative data points present in the truss design files that are produced at design time which can be used to [build and train a predictive model](https://docs.aws.amazon.com/machine-learning/latest/APIReference/API_Operations.html) for truss assembly time. But realized assembly times of past trusses is missing and required. 

The Shift Monitor App will accurately capture this missing data and provide a foundation for such a model.  

### Other Use Cases

#### Realtime Order Progress Information

Provides management and staff with real-time productivity information 

#### Factory Scheduling 

Provide accurate completion dates to customers and minimize unexpected overtime 

#### Performance Based Staff Bonuses

Measure, benchmark and reward staff productivity with high level of granularity 

#### Cost Based Optimization of Truss Design

Provide truss designers immediate and actionable feedback about their design at design time.

## Workflow

The general workflow at Cameo Truss begins when a potential customer submits a blueprint for a residential house and requests a quote for the trusses. The truss designer then uses software to design the required trusses. This design software produces text files from which many data points can be extracted. These design files are saved in subfolders in a Dropbox folder called "quotes". If a customer approves the quote and orders the trusses then the quote becomes a "job" and the trusses are scheduled for production by the truss plant. 

When a quote becomes a job the design files are copied from the "quotes" to another Dropbox folder called "jobs."

## Dataflow 

A Dropbox webhook is registered to an AWS API Gateway endpoint which triggers AWS Lambda function so that the [ETL process](https://en.wikipedia.org/wiki/Extract,_transform,_load) is triggered each time a file(s) changes on Dropbox (limited to once per minute).  This Lambda function [syncs the raw truss files in the Dropbox jobs folder to DynamoDB (details here).](https://docs.google.com/document/d/1n-lU0wnx9XNlKMB7mRH2iVEHjyssIazzliKdyUbL1Lg/edit?usp=drivesdk) A subsequent Lambda function then parses the truss file content and further populates the DynamoDB fields that populate the front end UI state. 

# Technology Stack

## [Material UI](https://material-ui-next.com/)

* Google's Material UI look and feel 

* React Component library

## [React JS](https://reactjs.org/) 

* Facebook's React js library

* React Router 

## [GraphQL](https://graphql.org/)

* Facebook's replacement for REST

* All client side data interaction is implemented with GraphQL 

## [Apollo GraphQL](https://www.apollographql.com/)

* GraphQL client library 

* Same purpose as Facebook's Relay

## [AWS AppSync](https://aws.amazon.com/appsync/) 

* GraphQL backend as a service 

* Connects a GraphQL schema to DynamoDB, Lambda, and ElasticSearch via resolvers which are defined with [Apache Velocity](http://velocity.apache.org/) syntax

* All client side GraphQL uses [AWS's Apollo](https://dev-blog.apollodata.com/aws-appsync-powered-by-apollo-df61eb706183) Middleware library to query to DynamoDB via AppSync 

## [AWS DynamoDB](https://aws.amazon.com/dynamodb/) 

* Nosql schema-less distributed database

* All app data is stored in DynamoDB tables

    * Backend Lambda function ETLs raw truss files from Dropbox into DynamoDB items

    * Frontend web app UI state is populated with data extracted from truss files

    * Frontend inserts truss assembly times

## [AWS Step Functions](https://aws.amazon.com/step-functions/)

* State machine that

* Allows Lambda functions to be composed 

## [AWS Lambda](https://aws.amazon.com/lambda/)

* Serverless function service

* Supports Java, node.js, and Python natively

    * Other languages supported via Node wrapper

* Dropbox webhook triggers ETL process of truss files stored in Dropbox into DynamoDB

    * ETL process is implemented with AWS Step Functions which orchestrate AWS Lambda Functions

    * Each Lambda function is designed to be 

## [AWS S3](https://aws.amazon.com/s3/)

* Amazon's AWS simple storage solution service

* Hosts web app as static website

## [Dropbox Java SDK](https://www.dropbox.com/developers/documentation/java) 

* Dropbox API v2 and webhook service

* Dropbox webhooks trigger ETL implemented with Lambda functions

## [AWS Route 53](https://aws.amazon.com/route53/)

* Domain name service

* Hosts custom domain name: [cameotruss.io](http://cameotruss.io) 

## Developer Tools

* [Circle CI](https://circleci.com/) for continuous integration 

* [Github](https://github.com/) for source control

* [Git Flow](https://github.com/nvie/gitflow) for workflow

* [AWS Cloud Formation](https://aws.amazon.com/cloudformation/) for (re)deployment 

* ??? for Java testing

* ??? for React testing

