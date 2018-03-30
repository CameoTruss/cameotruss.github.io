# Cameo Truss Shift Monitor App

## Technology Stack 
High level overview of technologies used.

### Front End
Single page web application
[Github repo](https://github.com/CameoTruss/ShiftMonitorFrontEnd)

#### Material UI
- Google's Material UI look and feel 
- React Component library

#### React 
- Facebook's React javascript library
- React Router 

#### Apollo
- Middleware connecting javascript UI to data
- GraphQL javascript client library 
- Same purpose as Facebook's Relay

### Back End
Serverless cloud
[Github repo](https://github.com/CameoTruss/ShiftMonitorBackEnd)

#### GraphQL
- Facebook's REST replacement data model 
- CRUD functionality

#### AppSync
- Amazon's AWS AppSync service
- Graphql version of API Gateway service
- Executes graphql resolvers
  - Apache Velocity syntax

#### Dynamodb 
- Amazon's AWS Dynamodb service
- Nosql schema less distributed database

### ETL

#### Lambda
- Amazon's AWS Lambda service 
- Serverless function service
- Support of Node.js allows you to wrap arbitrary binaries in javascript functions 
- ETL data from files stored in Dropbox into Dynamodb with Rust 

#### S3
- Amazon's AWS simple storage solution service

#### Dropbox 
- Dropbox's API v2 and webhook service
- Dropbox webhooks trigger ETL lambdas