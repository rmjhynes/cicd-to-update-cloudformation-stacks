# CI/CD Pipeline to Update CloudFormation Stacks
A demo project to create a deployment pipeline to create / update a CloudFormation stack using Amazon CodeCatalyst and AWS Cloud9.
Steps taken from here: [Build a CI/CD Pipeline to Improve Your IaC with AWS CloudFormation](https://community.aws/content/2dukZbCqB0wWgvV6VIioWVNJ959/build-ci-cd-pipeline-iac-cloudformation?lang=en)

## Technologies used
- Amazon CodeCatalyst used as an all in one development environment with features for code repositiories and automated workflows.
- AWS Cloud9 used as the browser IDE associated with CodeCatalyst project.
- AWS CloudFormation as the IaC service.

## High level process
- The cloudformation template is created / updated on all pushes to main.
- When PRs are opened, updated or revised targeting main (main is the destiantion branch to merge to) then a cloudformation changeset is created.
- Reviewer reviews the changeset in the AWS console and either merges or closes the PR.
- If merged (and therefore pushed to main) the cloudformation template is updated with changes outlined in the changeset.


## Step by step process
1. Configured CodeCatalyst IAM service roles by manually uploading cloudformation templates to my AWS account. This can be found in file 'CodeCatalyst-IAM-roles.json'.
2. Set up my codecatalyst space, project, repo and environment.
3. Set up dev environment using AWS Cloud9 IDE in browser.
4. Pushed 'cf-templates/VPC_AutoScaling_With_Public_IPs.json' cloudformation template to main.
5. Created workflow to create / update cloudformation template on pushes to main. This can be found in file 'codecatalyst/workflows/main_branch.yaml'.
6. Pushed this workflow to main thus triggering the workflow to create the cloudformation stack in my AWS account.
7. We now have a website! This can be seen in the 'Outputs' tab of cloudformation.
8. Created a workflow to create a cloudformation changeset whenever a PR is raised with the destination branch being main. This can be found in file 'codecatalyst/workflows/pr_branch.yaml'.
9. Created a new branch to update the cloudformation stack. The updates can be found in 'cf-templates/VPC_AutoScaling_With_Public_IPs-3-subnets.json'.
10. Pushed these changes to remote and then created a pull request. The PR triggered the 'PR_Branch_Workflow' workflow and thus created a cloudformation changeset.
11. I, as the special reviewer, reviewed the changeset and thought it looked great!
12. Merged the PR with main which then kicked off the main workflow to update the cloudformation stack with the changes outlined in the changeset.

