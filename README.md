# How To Doc
## Creation of CodePipeline triggered from Github Repo events
This process will get you set up with a codepipeline that will
pull code from Github when a commit/pr event occurs on the
master branch of a specified repo. The pipeline will then move
the code into a defined s3 bucket. If you want it to deploy
cfn, then add a deploy step; if you want it to run awscli
commands, just add to the build step.

## Pre-Reqs
- (Github account)[https://github.com/TylerFilko/test_repo1/tree/master/codepipline_demo/Github_Howto.md]
- (An AWS account)[https://github.com/TylerFilko/test_repo1/tree/master/codepipline_demo/AWS_Setup.md]
- A user role with admin access
- A role with admin access
- A way to assume this role
- docker installed
- SSM parameter containing a github developer token stored in the path /my_cfn_pipeline/github.token as a string; do not store as a secret string, cfn sucks at pulling these (you can limit there permissions to only one repo)
- SSM string parameter containing the ssm path where the github token is stored (this is useful if you need a script to pull the secret later as codepipeline will pass a token around in plain text if you need it in other pipeline steps, just keeps your stuff from being in debug logs)

## Steps
1. Create a private github repo (and yes private is a must)
2. Copy the codepipline_demo directory into your repo
3. Replace all fields within `<replace_txt_description>`
4. Run the Makefile command `Make deploy-cfn` using user token
5. Go into the AWS console -> cloudformation . Here you should see your stack deployed. This stack should contain your Codepipeline, Codebuild, s3 Bucket, and roles.
6. Now update your github repo. If you got back to AWs -> Codepipeline you should be able to see that the commit to master triggered your build. Try going to your s3 bucket, your git repo should now be in s3!
7. If you want your Build to action on your code (ie. run awscli commands) you can alter the buildspec.yml in the deployment directory. In the case that you want to add a Cloudformation deployment, look to the additions.cfn file for examples.
8. Enjoy
