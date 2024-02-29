# aws-cicd-serverless

This solution provides a developer with a basic CICD pipeline using CodePipeline, CodeCommit, CodeBuild, CloudFormation. Some steps below.

First clone this repository:
```
git clone https://github.com/kbdraai/aws-cicd-serverless.git
cd aws-cicd-serverless/
```

Then remove the .git folder and deploy the bootstrap.yml CloudFormation template on the console. Run the following:
```
rm -rf .git/
```

Then deploy the bootstrap.yml Cloudformation template. You can do it on the console. Change the parameters to your own values (especially the artefact bucket as bucket names are unique across a region).

Go to the resources in your CloudFormation stack once done and navigate to the CodeCommit repository. LogicalID: VersionControl
Set up your Git credentials here: https://docs.aws.amazon.com/codecommit/latest/userguide/setting-up-gc.html?icmpid=docs_acc_console_connect_np

Update the buildspec.yml with the name of the artefact bucket you chose: 
```
--s3-bucket <artifacts_bucket_name>
```

Move files into CodeCommit repo:
```
git init
git add .
git commit -m "first commit"

git remote add origin <code_commit_url>
git push origin main
```

Then go to the CodePipeline created on the CodePipeline console and run it again. All the stages should succeed now.

Go to the CloudFormation template in the DeploymentStage. Then Resources then select the function. When testing it should return:
```
{
  "statusCode": 200,
  "body": "{\"message\": \"hello world\"}"
}
```

You can now make changes to the Lambda function on your machine and push it. Upon pushing it this should kick off a new pipeline execution.
