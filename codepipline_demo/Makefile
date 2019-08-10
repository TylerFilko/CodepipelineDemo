deploy-cfn-container:
	docker build -t deploy-ta-codebuild -f $(shell pwd)/Dockerfile.deploycfn .

deploy-cfn: deploy-cfn-container
	docker run --rm -it \
			-v $(shell pwd):/src \
			deploy-ta-codebuild \
			aws --region us-east-1 cloudformation create-stack \
				--template-body file://src/codepipeline_cfn.json \
				--stack-name my-cfn-code-pipeline \
				--capabilities CAPABILITY_NAMED_IAM \
				--tags Key=project,Value=cloudformation_pipeline_demo

update-cfn: deploy-cfn-container
	docker run --rm -it \
			-v $(shell pwd):/src \
			deploy-ta-codebuild \
			aws --region us-east-1 cloudformation update-stack \
				--template-body file://src/codepipeline_cfn.json \
				--stack-name my-cfn-code-pipeline \
				--capabilities CAPABILITY_NAMED_IAM \
				--tags Key=project,Value=cloudformation_pipeline_demo
