# Concepts 

## StrataApp
This is a CDK application that is executed in an account. It takes in **StrataConfig** and generates and deploys CloudFormation Stacks. 
A StrataApp consists of a number of components:
### StrataStack 
This is an abstraction of a CDK Stack. It handles the management of config and exposes it to the application. It is represented by a single CloudFormation stack. 

### StrataConstruct
This is an abstraction of a CDK Construct. It interacts with the StrataStack to ensure the relevant config is used by the app. Inside here is primarily CDK Constructs of any level.  These are represented by a single or multiple CFn Resources inside an individual Cfn Stack

## StrataConfig
Is an config file that represents a collection of VPCs inside multiple accounts, either grouped by OU or another property. We primarily group by OU so that is the most common use case for us here. 

## StratumConfig
This is the single definition that defines a VPC and any resources inside it. It can obtain defaults value from labeled or default config blocks inside the StrataConfig file. 

## StrataPipelines
These are based on [ADF](../adf/understanding-adf.md) pipelines. They are responsible for orchestrating builds and invoking StrataApps inside the relevant account. 
It's important to note, that although it is designed to deploy IaC into any account. Strata pipelines operate in eu-west-2. This region servers as our relative dimension regardless of account or strata application and where possible global resources will be deployed in this region. 

