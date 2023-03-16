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
9.Wait for CodePipeline to finish. This should take around 2 minutes. 
11. Once completed, you can click on "Details" in the "CheckovTestAction" box to check the Checkov scan results. 
<img alt="image" src="https://user-images.githubusercontent.com/126644393/225557374-54afd4d2-b238-423a-8cff-a23b72f749e9.png">
12. The results will include number of passed checks and failed checks, and line numbers of the infrastructure code that failed specific policy. The sample CloudFormation template should not have any vulnerabilities and the vulnerability stage should succeed without any failures.
<img alt="image" src="https://user-images.githubusercontent.com/126644393/225558759-9e572290-7d9a-4617-8c0b-f3cbf8e93521.png">


## Scenario 1 - EC2 instance with public IP, unencrypted disk and open security group

### Scan vulnerable EC2 template
1. From this project download the **ec2.yaml**
2. Navigate to you CodeCommit Repository in AWS console
![image](https://user-images.githubusercontent.com/126644393/225547486-03187fe0-e016-42c6-8ac9-3a992731b225.png)
3. Select 'Add file' then "Upload file", choose the  **ec2.yaml** file you just downloaded and click "Commit changes"
![image](https://user-images.githubusercontent.com/126644393/225547267-a4a946f0-7535-4df0-9ded-fe2b95e22ca1.png)
4. Wait for CodePipeline to run and detect vulnerabilities. This should take around x minutes.
5. Once completed, check the completion status of the 'Test Action'. It should result in a 'Failed' state.
6. Click on "View in CodeBuild" to see the details of the vulnerabilities

### Fix vulnerabilities in EC2 template
1. Navigate to you CodeCommit Repository in AWS console
2. Select the **ec2.yaml** file then click 'Edit'
3. Override the original content with the **ec2-fixed.yaml** file from this project and click "Commit changes"
4. Wait for CodePipeline to run and detect vulnerabilities. This should take around x minutes.
5. Once completed, check the completion status of the 'Test Action'. It should result in a 'Succeeded' state
6. Click on "View in CodeBuild" to see the details of the scan results

## Scenario 2 - S3 bucket with public access and incoming traffic from any IP address

### Scan vulnerable S3 template
1. From this project download the **s3.yaml** and the static website files
2. Navigate to you CodeCommit Repository in AWS console
3. Select 'Add file' then "Upload file", choose the  **s3.yaml** file you just downloaded and click "Commit changes"
4. Upload the static website files to the responsitory
5. Wait for CodePipeline to run and detect vulnerabilities. This should take around x minutes.
6. Once completed, check the completion status of the 'Test Action'. It should result in a 'Failed' state
7. Click on "View in CodeBuild" to see the details of the vulnerabilities

### Fix vulnerabilities in S3 template
1. Navigate to you CodeCommit Repository in AWS console
2. Select the **s3.yaml** file then click 'Edit'
3. Override the original content with the **s3-fixed.yaml** file from this project and click "Commit changes"
4. Wait for CodePipeline to run and detect vulnerabilities. This should take around x minutes.
5. Once completed, check the completion status of the 'Test Action'. It should result in a 'Succeeded' state.
6. Click on "View in CodeBuild" to see the details of the scan results

## Scenario 3 - Essential 8 control violations

### Scan vulnerable DynamoDB template
1. From this project download the **dynamoDB.yaml** and the static website files
2. Navigate to you CodeCommit Repository in AWS console
3. Select 'Add file' then "Upload file", choose the  **dynamoDB.yaml** file you just downloaded and click "Commit changes"
4. Upload the static website files to the responsitory
5. Wait for CodePipeline to run and detect vulnerabilities. This should take around x minutes.
6. Once completed, check the completion status of the 'Test Action'. It should result in a 'Failed' state
7. Click on "View in CodeBuild" to see the details of the vulnerabilities

### Fix vulnerabilities in DynamoDB template
1. Navigate to you CodeCommit Repository in AWS console
2. Select the **dynamoDB.yaml** file then click 'Edit'
3. Override the original content with the **dynamoDB-fixed.yaml** file from this project and click "Commit changes"
4. Wait for CodePipeline to run and detect vulnerabilities. This should take around x minutes.
5. Once completed, check the completion status of the 'Test Action'. It should result in a 'Succeeded' state.
6. Click on "View in CodeBuild" to see the details of the scan results

## Scenario 4 - Custom policies

### Scan vulnerable AppSync template
1. From this project download the **dynamoDB.yaml** and the static website files
2. Navigate to you CodeCommit Repository in AWS console
3. Select 'Add file' then "Upload file", choose the  **dynamoDB.yaml** file you just downloaded and click "Commit changes"
4. Upload the static website files to the responsitory
5. Wait for CodePipeline to run and detect vulnerabilities. This should take around x minutes.
6. Once completed, check the completion status of the 'Test Action'. It should result in a 'Failed' state
7. Click on "View in CodeBuild" to see the details of the vulnerabilities

### Fix vulnerabilities in AppSync template
1. Navigate to you CodeCommit Repository in AWS console
2. Select the **AppSyncAPI.yaml** file then click 'Edit'
3. Override the original content with the **AppSyncAPI-fixed.yaml** file from this project and click "Commit changes"
4. Wait for CodePipeline to run and detect vulnerabilities. This should take around x minutes.
5. Once completed, check the completion status of the 'Test Action'. It should result in a 'Succeeded' state.
6. Click on "View in CodeBuild" to see the details of the scan results

## Additional Exercises

### Import vulnerabilities to Security Hub


From this project download the **lambda.zip** and import to Lambda. The lambda function will be used with the DevSecOps pipeline to export the findings to SecurityHub.

### Use CodePipeline to deploy resources in Scenario 1-4
