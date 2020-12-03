# A Brief History of Strata

## v0
Monolithic app used to create globe poc account. It was a first time using CDK and we proved the concept worked.  

## v1 
- First iteration deployed with ADF. 
- ADF Pipeline defined with Jinja templated JSON
- Numerous repos all implementing the same logic for building resources in relevant availability zones 
- Lambdas required dependencies to be vended in the repository 
- Each repository has its own buildspec

## v2 
- Introduction of the StrataApp, centralised config managements and abstractions of per_account and per_az
- New StrataStack and StrataConstructs classes that provide some consistency in accessing stratum config
- Buildspec deprecated and moved into CodeBuild projects

## v3
- StrataApp introduces per_account, per_region and per_az functions.
- Allows for multi-region account configuration
- BuildSpec further deprecated and replaced with strata-cli ci tooling. 

## v4
- TBC 

## v5
- TBC
