In order to run the pipeline generation locally. You really need to replacate the behaviour of the CodeBuild project in the Account Orchestration account. 
This can be done my running the following commands when logged in as INFX_access in the Account Orchestration account.


```
aws s3 cp s3://adf-shared-modules-eu-west-2-ft3wq6/adf-build/ ./adf-build/ --recursive
pip install -r adf-build/requirements.txt  -t ./adf-build
chmod 755 adf-build/cdk/execute_pipeline_stacks.py adf-build/cdk/generate_pipeline_inputs.py adf-build/cdk/generate_pipeline_stacks.py
export AWS_REGION=eu-west-2
export ACCOUNT_ID=433612406028
export S3_BUCKET_NAME=adf-global-base-deployment-pipelinebucket-13k5mriz57hga
export MASTER_ACCOUNT_ID=914779079504
export ORGANIZATION_ID=o-g9hwpfycbb
export ADF_PIPELINE_PREFIX=adf-pipeline-
export ADF_STACK_PREFIX=adf-
export ADF_LOG_LEVEL=DEBUG
export ADF_VERSION=local
python adf-build/cdk/generate_pipeline_inputs.py
export SHARED_MODULES_BUCKET=adf-shared-modules-eu-west-2-ft3wq6
export STRATA_CONFIG_BUCKET=skyscanner-strata-config-store
python adf-build/cdk/generate_pipeline_inputs.py

python adf-build/cdk/generate_pipeline_inputs.py
cdk synth --app adf-build/cdk/generate_pipeline_stacks.py
cdk synth --app adf-build/cdk/generate_pipeline_stacks.py
```
