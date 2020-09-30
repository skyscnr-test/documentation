## AWS Deployment Framework (ADF)
### Concepts 

#### Deployment Maps
Deployment maps are defined in the aws-deployment-framework-pipelines repository. 
Simply put the represent a pipeline and configure what actions it carries out in what accounts. 

It defines the *Source*, *Build* and *Deploy* actions and specifies the *targets* 

##### Source 
The source action maps directly to the Source stage of a CodePipeline. 
It can be used to configure the source of the repository as CodeCommit, GitLab, S3 or Github.com 

Currently within Skyscanner, we only use CodeCommit. 

##### Build 
The build stage maps to a Build action type in CodePipeline. 
It can be used to configure CodeBuild or Jenkins actions. 

At Skyscanner we **only** use CodeBuild

##### Deploy 
This is the most configurable option. It can be used to invoke lambdas, trigger CodeBuilds, activate CodeDeploy and deploy CloudFormation natively.
Depending on the pipeline type, the actions will happen within the context of the target accounts via assumed roles. 

#### Targets
Simply put targets == accounts. In deployment maps, we can specify accounts direct by account number, by tag or by their Organization Unit (OU)
Then will then be added to the pipeline in the order specified in the targets list. 


#### Bootstrap Templates
Bootstrap templates are used to create consistent accounts. 
They should really be used for the bare minimum and any modifications to it should be considered by other means. For example we bootstrap every account with the necessary roles to run ADF. 

We then use ADF to bootstrap the necessary roles and projects to use Strata inside any accounts. 


### How it Works

## Master Billing Account / OU master account
This account is used to detect any changes to the OU structure and create new accounts. 
When an OU event is detected, it is used to trigger CodeBuild, this will bootstrap the accounts with the required bootstrap template. 
Once complete it will ensure all accounts are tagged as per their definition and then trigger a StateMachine in the Deployment Account

## Account Orchestration Account / Deployment Account 
This account is responsible for holding all the CodePipelines, CodeCommit repositories and all other deployment related objects. 
Inside this account is the crucial adf-deployment-pipelines-pipeline. 

This pipeline is responsible for processing and creating pipelines defined in the deployment map(s). It's proabbaly worth talking abit about how it works. 

### Pipelines Pipeline
This pipeline is relatively simple. It consists of a Source which is CodeCommit (aws-deployment-framework-pipelines) and a CodeBuild stage. 
the Codebuild stage has a simple BuildSpec that 
- Generates Pipeline input 
    This is a process that converts the deployment-map definitions into JSON files. 
- Generates Pipeline Stacks
    This runs a CDK synth of stacks that load in the JSON files above to generate CodePipelines and their targets. 
- Execute Stacks
    This stage deploys the synthised template via CloudFormation. 
- Clean Pipelines
    This final step runs as a POST_BUILD step. It processes the deployment maps and removes any pipelines that no longer exist in that account. 



