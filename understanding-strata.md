## What Is Strata?
Strata is a framework that is responsible for carrying out two functions at Skyscanner. These are (1) Provisioning New Accounts and (2) Deploying and maintaining core infrastructure across these accounts. 

## How is it deployed?

Strata is itself bootstrapped by the AWS Deployment Framework(ADF). ADF is responsible for deploying the underlying infrastructure used by Strata in order to function. This ranges from S3 Buckets, KMS Keys and IAM roles in the account-orchestration account to CodeBuild Projects used by CodePipline in the target accounts. 

In order to use Strata in an account, that account must be tagged with ```strata_enabled: true```

## How does it work? 
The theory behind Strata is quite simply. A Strata Application (Or Stratum) is pushed to CodeCommit in the account-orchestration account. From here it triggers a CodePipeline that will push the Stratum to a CodeBuild project in the target accounts, this CodeBuild project then executes the Stratum, generating CloudFormation templates that can be deployed in the target account. 

The framework is designed to create CloudFormation that is ultimately account, region and availabity-zone agnostic. 

## How can I get started?
There's a [getting started guide](getting-started.md) that will talk you through getting your first application deployed. 
There's also more information in the [strata directory](strata/)