## Waves instead of Targets
Currently our local build has the concept of waves to compliment targets. 
In the upstream build. Pipeline Input is generated with targets represented as a list of account ids 
Target = [123, 456, 789]

In our local build we have the concept of waves
Target = [[123, 456, 789], [321, 654, 987]]

In the pipeline generation stage, these waves will try and fit into the same stage as sequential actions. Where that isn't possible ADF will create a pipeline with multiple stages associated with the same target. 