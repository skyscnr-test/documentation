# Foreword
Strata is a framework that is responsible for carrying out two functions at Skyscanner. These are (1) Provisioning New Accounts and (2) Deploying and maintaining core infrastructure across these accounts. 

There are many parts to how it achieves its goal. These are the following: A Strata App, individual python3 apps that define CDK Applications. These apps are enhanced by using the StrataCore library. A custom python library that provides a standardised process for the injection and consumption of a YAML config file that defines how an account looks. (Strata Config). These Strata Apps are then executed in each target account via a combination of CodeCommit, CodePipeline, CodeBuild and S3. (This functionality is provided by the AWS Deployment Framework, ADF) 

# Why ADF, Why CDK? Why not X, Y or Z?
This was a big decision for Cassini at the start of Project Globe. Previously up until this time, accounts had been provisioned by Terraform and deployed to manually, from a squad members laptop. (Eek) When we made the decision, we started with a clean slate. (What got us here, won't get us there.) We began with making a model of what we wanted, in an ideal world, how would we deploy our infrastructure to accounts. 


We looked first at a delivery mechanism. How could we deploy infrastructure to each account? We considered managed solutions for deploying Terraform, we considered Spinnaker pipelines, we even looked at AWS Provided services: Control Tower, Landing Zone, Stack Sets. All of had benefits, but all of them had problems. We then came across the AWS Deployment Framework. (An Open Source solution provided by AWS Labs). It solved alot of our problems. It fitted with our mental model of how we wanted to deploy X to our accounts. (We still hadn't chosen CDK for it at this point) but it had support for Terraform, CloudFormation and CDK. From there we discussed with TAMS and SAs about out problem. We wanted to be able to create multiple accounts from a common template, that was also slightly modifiable and still easy to maintain. We wanted something that was accessible by the entire company and wouldn't create a silo of knowledge. The goal was to push core infrastructure to the left and enable other tribes and squads within the company to deploy and run it themselves. 

CDK was chosen, because it is an AWS supported product, we have close ties to developers and developer advocates on the team and it produces CloudFormation at the end of the day. It can be used in Python, Java, Node and C#. We've opted to implement it in Python because it's a Skyscanner supported language (and we currently have no plans to change this)  


CDK also allows us to implement guidelines, common libraries and other standards and dynamically generate (at times complicated) CloudFormation via Python. Widely known and worked with within Skyscanner. 

So we now have Infrastructure defined as Code IaC has primarily up until the point been treating Infrastructure as Code. Now we have it as code. We can start to do other things with it. Unit Tests for one. Strata has introduced with it unit tests for core infrastructure. We can now tell if the code we've changed will critically change an IAM Role which also has a benefit of improving our security posture. 



## Why are you running X,Y,Z when squad X provides a similar service?
### Artifactory
Inside the strata ecosystem, we opt not to use Skyscanner's own artifactory offering for managing python packages. Instead we use a lightweight offering called s3pypi, this allows us to publish artifacts that can be read from every account in our organization. It also means that we reduce our dependencies on external squads, and reduce the impact of outages. For example: This outage would have resulted in a situation where we were unable to push core infrastructure to any of our accounts. 

### Github
Any repositories in Strata write-through Github to AWS CodeCommit. We use / prefer CodeCommit because it integrates more efficiently with the AWS CodeStar Suite. Yet we retain the more familiar UI of Github. It also allows us to "Break Glass" and bypass Github in the event of an outage. 



## Why should X,Y,Z use Strata to deploy. 
### Consistency
Strata allows us to deploy infrastructure consistently to all strata managed accounts, it allows for small modifications per region / accounts (For example, AMI IDs, AZ Ids) but the underlying infrastructure is defined in the same fashion. 

### Rapid Expanse to new accounts / regions
By adding one section of YAML to an account definition, a properly written Strata App will automatically be deployed to any new regions / accounts created. We can also leverage AWS Custom Resources to hook into other processes as part of account provisioning. If you've got functionality that has to be carried out manually when we create new accounts / regions Strata could help solve this issue.

### Disaster Recovery
Both of the above features go hand in hand to provide us with a greater ability to recover from an account wide disaster, ADF will automatically provision accounts if they no longer exist / the name changes, this allows us to quickly re-provision accounts with the same core infrastructure. 







