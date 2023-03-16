# CyberCon 2023 - Integrating Security and Compliance in Infrastructure-as-Code Workshop 

# Table of Contents

1. [Workshop Instructions](#workshop-instructions)
2. [Lab Envrionment Setup](#lab-envrionment-setup)
3. [Initialise Lab](#Initialise-Lab)
4. [Check Pipeline](#Check-Pipeline)
5. [Scenario 1 - EC2 instance with public IP, unencrypted disk and open security group](#scenario-1---ec2-instance-with-public-ip-unencrypted-disk-and-open-security-group)
6. [Scenario 2 - S3 bucket with public access and incoming traffic from any IP address](#scenario-2---s3-bucket-with-public-access-and-incoming-traffic-from-any-ip-address)
7. [Scenario 3 - Essential 8 control violations](#scenario-3---essential-8-control-violations)
8. [Scenario 4 - Custom policies](#scenario-4---custom-policies)
9. [Additional Exercises](#additional-exercises)

## Workshop Instructions
Welcome to "Integrating Security and Compliance in Infrastructure-as-Code" workshop!
In this workshop you will learn to incorporate security guard rials in the CI/CD Pipeline to deliver infrastructure code with high bar of security considerations.

This workshop includes a DevSecOps CI/CD pipeline to deploy cloud resources. You will use AWS services to provide the core CI/CD platform, and an open-source tool Checkov to perform the vulnerability scanning of the infrastructure code.

## Lab Envrionment Setup
In this section, you will deploy the DevSecOps pipeline and initiate it. You will use the CloudFormation templates provided in each scenario to trigger the pipeline.

For building the CI/CD pipeline, you will use AWS managed services “AWS CodeCommit”, “AWS CodeBuild”, and “AWS CodePipeline”. For additional information on AWS DevOps tools and services, please refer to [AWS DevOps](https://aws.amazon.com/devops/).

[AWS CodeCommit](https://aws.amazon.com/devops/) – A fully managed source control service that hosts secure Git-based repositories.
[AWS CodeBuild](https://aws.amazon.com/codebuild/) – A fully managed continuous integration service that compiles source code, runs tests, and produces software packages that are ready to deploy.
[AWS CodePipeline](https://aws.amazon.com/codepipeline/) – A fully managed continuous delivery service that helps you automate your release pipelines for fast and reliable application and infrastructure updates.

Below is the architecture diagram of our DevSecOps pipeline. The diagram includes a CloudFormation Stack Instance that deploys the cloud recourses to an AWS account. For simplicity, in this workshop we will just perform vulnerability scanning of the infrastructure code. You can follow the instructions in  [Additional Exercises](#additional-exercises) to add deployment and other features to the DevSecOps pipeline in your own time. 

![IaC DevSecOps Pipeline Architecture-Lab](https://user-images.githubusercontent.com/126644393/225544261-9257d471-d532-4ddc-9399-86b992f1c3c3.png)

## Initialise Lab
Let’s deploy the DevSecOps pipeline using the CloudFormation template provided in this project.

From the "environment" folder download the **devsecops-pipeline.yaml** and deploy the pipeline in CloudFormation console.

Once the CloudFormation template is deployed successfully, you will see the DevSecOps pipeline under AWS CodePipeline. Below is the screenshot of the pipeline after a successful execution. Please note, you may see a failed status because you haven’t initiated your pipeline yet.

![image](https://user-images.githubusercontent.com/126644393/225551274-6661ae1f-7826-45f1-9188-50df5e54ac49.png)

## Check Pipeline

Let’s upload a sample CloudFormation script to CodeCommit to test the CodePipeline.

1. From the "environment" folder download the **sample-template.yaml** and 
2. Go to CodeCommit following [this link](https://ap-southeast-2.console.aws.amazon.com/codesuite/codecommit/repositories/cloudformation-checkov-test/browse?region=ap-southeast-2)
3. Select 'Add file' then "Upload file", choose the  **sample-template.yaml** file you just downloaded and click "Commit changes"
![image](https://user-images.githubusercontent.com/126644393/225547267-a4a946f0-7535-4df0-9ded-fe2b95e22ca1.png)

4. This will automatically trigger the pipeline and go through the vulnerability scanning stages. You can go to [this link](https://ap-southeast-5.console.aws.amazon.com/codesuite/codepipeline/pipelines/iac-devsecops-pipeline/view?region=ap-southeast-2) to check the running status of your pipeline.
6. If your pipeline does not start automatically, click on the "Edit" button from the top right of the CodePipeline screen, click on "Edit Stage" of the Source Action stage, then click on the edit icon in the "SourceCodeAction" box.
![image](https://user-images.githubusercontent.com/126644393/225553148-e15e52a5-3067-474b-a055-95c42004e881.png)
In the popup window, clear the Branch name by clicking on the cross icon next to it, and select "main" from the dropdown again. This is to ensure any changes on the "main" branch will trigger the CodePipeline.
![image](https://user-images.githubusercontent.com/126644393/225553662-8cd0061b-eefe-460b-92e4-17cb9e3d70ba.png)
7. Click on "Done" and save the changes
8. click on the "Release change" button from the top right of the CodePipeline screen to manually trigger the pipeline with the most recent change.
![image](https://user-images.githubusercontent.com/126644393/225553148-e15e52a5-3067-474b-a055-95c42004e881.png)

9. Wait for CodePipeline to finish. This should take around 2 minutes. 
10. Once completed, you can click on "Details" in the "CheckovTestAction" box to check the Checkov scan results. 
<img alt="image" src="https://user-images.githubusercontent.com/126644393/225557374-54afd4d2-b238-423a-8cff-a23b72f749e9.png">

11. The results will include number of passed checks and failed checks, and line numbers of the infrastructure code that failed specific policy. The sample CloudFormation template should not have any vulnerabilities and the vulnerability stage should succeed without any failures.
<img alt="image" src="https://user-images.githubusercontent.com/126644393/225558759-9e572290-7d9a-4617-8c0b-f3cbf8e93521.png">


## Scenario 1 - EC2 instance with public IP, unencrypted disk and open security group

### Scan vulnerable EC2 template
1. Download the files from the "scenario-1" folder
2. Navigate to you CodeCommit Repository in AWS console following [this link](https://ap-southeast-2.console.aws.amazon.com/codesuite/codecommit/repositories/cloudformation-checkov-test/browse?region=ap-southeast-2)
3. Select the **sample-template.yaml** file then click 'Edit'
4. Override the original content with the **ec2.yaml** file you just downloaded and click "Commit changes"
![image](https://user-images.githubusercontent.com/126644393/225560291-5d6fccbd-2cb4-4a50-a686-e6f4b7a9241b.png)

4. Wait for CodePipeline to run and detect vulnerabilities.
5. Once completed, check the completion status of the 'CheckovTestAction' box. It should result in a 'Failed' state.
6. Click on "View in CodeBuild" to see the details of the vulnerabilities
<img alt="image" src="https://user-images.githubusercontent.com/126644393/225561838-7210a167-e127-484f-8dda-ea01d8a0b9e7.png">
7. The failed checks are:
- Check: CKV_AWS_3: "Ensure all data stored in the EBS is securely encrypted"
- Check: CKV_AWS_88: "EC2 instance should not have public IP."
- Check: CKV_AWS_260: "Ensure no security groups allow ingress from 0.0.0.0:0 to port 80"
- Check: CKV_AWS_24: "Ensure no security groups allow ingress from 0.0.0.0:0 to port 22"
8. Next we will fix these issues in our CloudFormation template and upload the fixed version to CodeCommit Repository

### Fix vulnerabilities in EC2 template
1. Navigate to you CodeCommit Repository in AWS console
2. Select the **sample-template.yaml** file then click 'Edit'
3. Override the original content with the **ec2-fixed.yaml** file you just downloaded and click "Commit changes"
4. Wait for CodePipeline to run and detect vulnerabilities. 
5. Once completed, check the completion status of the 'CheckovTestAction' box. It should result in a 'Succeeded' state
6. Click on "View in CodeBuild" to see the details of the scan results


## Scenario 2 - S3 bucket with public access and incoming traffic from any IP address

### Scan vulnerable S3 template
1. Download the files from the "scenario-2" folder
2. Navigate to you CodeCommit Repository in AWS console
3. Select the **sample-template.yaml** file then click 'Edit'
4. Override the original content with the **s3.yaml** file you just downloaded and click "Commit changes"
6. Wait for CodePipeline to run and detect vulnerabilities. 
7. Once completed, check the completion status of the 'CheckovTestAction' box. It should result in a 'Failed' state
8. Click on "View in CodeBuild" to see the details of the vulnerabilities
9. The failed checks are:
- Check: CKV_AWS_21: "Ensure the S3 bucket has versioning enabled"
- Check: CKV_AWS_56: "Ensure S3 bucket has 'restrict_public_bucket' enabled"
- Check: CKV_AWS_20: "Ensure the S3 bucket does not allow READ permissions to everyone"
- Check: CKV_AWS_55: "Ensure S3 bucket has ignore public ACLs enabled"
- Check: CKV_AWS_54: "Ensure S3 bucket has block public policy enabled"
- Check: CKV_AWS_53: "Ensure S3 bucket has block public ACLS enabled"
- Check: CKV_AWS_18: "Ensure the S3 bucket has access logging enabled"
10. Next we will fix these issues in our CloudFormation template and upload the fixed version to CodeCommit Repository

### Fix vulnerabilities in S3 template
1. Navigate to you CodeCommit Repository in AWS console
2. Select the **sample-template.yaml** file then click 'Edit'
3. Override the original content with the **s3-fixed.yaml** file you just downloaded and click "Commit changes"
4. Wait for CodePipeline to run and detect vulnerabilities. 
5. Once completed, check the completion status of the 'CheckovTestAction' box. It should result in a 'Succeeded' state.
6. Click on "View in CodeBuild" to see the details of the scan results


## Scenario 3 - Essential 8 control violations
In this scenario, we will scan a CloudFormation template that violates 2 coontrols from Essential 8 framework. The CloudFormation template will deploy a DynamoDB database without KMS encryption and backup.

### Scan vulnerable DynamoDB template
1. Download the files from the "scenario-3" folder
2. Navigate to you CodeCommit Repository in AWS console
3. Select the **sample-template.yaml** file then click 'Edit'
4. Override the original content with the **dynamoDB.yaml** file you just downloaded and click "Commit changes"
6. Wait for CodePipeline to run and detect vulnerabilities. 
7. Once completed, check the completion status of the 'CheckovTestAction' box. It should result in a 'Failed' state
8. Click on "View in CodeBuild" to see the details of the vulnerabilities
9. The failed checks are:
- Check: CKV_AWS_119: "Ensure DynamoDB Tables are encrypted using a KMS Customer Managed CMK"
- Check: CKV_AWS_28: "Ensure Dynamodb point in time recovery (backup) is enabled"
10. Next we will fix these issues in our CloudFormation template and upload the fixed version to CodeCommit Repository

### Fix vulnerabilities in DynamoDB template
1. Navigate to you CodeCommit Repository in AWS console
2. Select the **dynamoDB.yaml** file then click 'Edit'
3. Override the original content with the **dynamoDB-fixed.yaml** file you just downloaded and click "Commit changes"
4. Wait for CodePipeline to run and detect vulnerabilities.
5. Once completed, check the completion status of the 'CheckovTestAction' box. It should result in a 'Succeeded' state.
6. Click on "View in CodeBuild" to see the details of the scan results


## Scenario 4 - Custom policies
In this scenario, 

### Scan vulnerable AppSync template
1. Download the files from the "scenario-4" folder
2. Navigate to you CodeCommit Repository in AWS console
3. Select the **sample-template.yaml** file then click 'Edit'
4. Override the original content with the **appsyncapi.yaml** file you just downloaded and click "Commit changes"
5. Upload the static website files to the responsitory
6. Wait for CodePipeline to run and detect vulnerabilities.
7. Once completed, check the completion status of the 'CheckovTestAction' box. It should result in a 'Failed' state
8. Click on "View in CodeBuild" to see the details of the vulnerabilities
9. The failed checks are:
- Check: CKV_AWS_119: "Ensure DynamoDB Tables are encrypted using a KMS Customer Managed CMK"
- Check: CKV_AWS_28: "Ensure Dynamodb point in time recovery (backup) is enabled"
10. Next we will fix these issues in our CloudFormation template and upload the fixed version to CodeCommit Repository

### Fix vulnerabilities in AppSync template
1. Navigate to you CodeCommit Repository in AWS console
2. Select the **sample-template.yaml** file then click 'Edit'
3. Override the original content with the **appsyncapi-fixed.yaml** file you just downloaded  and click "Commit changes"
4. Wait for CodePipeline to run and detect vulnerabilities. 
5. Once completed, check the completion status of the 'CheckovTestAction' box. It should result in a 'Succeeded' state.
6. Click on "View in CodeBuild" to see the details of the scan results


## Additional Exercises

### Import vulnerabilities to Security Hub
1. Download the files from the "additional-exercises" folder
2. Import the **ImportVulToSecurityHub.zip** file to Lambda. The lambda function will be used with the DevSecOps pipeline to export the findings to SecurityHub.
3. Upload the **buildspec-checkov.yml** file to your CodeCommit Repository

### Use CodePipeline to deploy resources in Scenario 1-3
