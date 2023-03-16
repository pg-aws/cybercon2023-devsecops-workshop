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

From this project download the **lambda.zip** and import to Lambda. The lambda function will be used with the DevSecOps pipeline to export the findings to SecurityHub.

For building the CI/CD pipeline, you will use AWS managed services “AWS CodeCommit”, “AWS CodeBuild”, and “AWS CodePipeline”. For additional information on AWS DevOps tools and services, please refer to [AWS DevOps](https://aws.amazon.com/devops/).

[AWS CodeCommit](https://aws.amazon.com/devops/) – A fully managed source control service that hosts secure Git-based repositories.
[AWS CodeBuild](https://aws.amazon.com/codebuild/) – A fully managed continuous integration service that compiles source code, runs tests, and produces software packages that are ready to deploy.
[AWS CodePipeline](https://aws.amazon.com/codepipeline/) – A fully managed continuous delivery service that helps you automate your release pipelines for fast and reliable application and infrastructure updates.

Below is the architecture diagram of our DevSecOps pipeline. The diagram includes a CloudFormation Stack Instance that deploys the cloud recourses to an AWS account. For simplicity, in this workshop we will just perform vulnerability scanning of the infrastructure code. You can follow the instructions in  [Additional Exercises](#additional-exercises) to add deployment and other features to the DevSecOps pipeline in your own time. 

![IaC DevSecOps Pipeline Architecture-Lab](https://user-images.githubusercontent.com/126644393/225544261-9257d471-d532-4ddc-9399-86b992f1c3c3.png)

## Initialise Lab
From this project download the **CheckovPipeline.yaml** and deploy the pipeline in CloudFormation console.

Once the CloudFormation template is deployed successfully, you will see the DevSecOps pipeline under AWS CodePipeline. Below is the screenshot of the pipeline after a successful execution. Please note, you may see a failed status because you haven’t initiated your pipeline yet.

## Check Pipeline

Let’s make a minor change to our sample CloudFormation script and push it to CodeCommit

Go to CodeCommit following this link and take a look at our Git repository, it should look like this at this point:

This will automatically trigger the pipeline and go through the vulnerability scanning stages. The sample CloudFormation tempalte should not have any vulnerabilities and the vulnerability stage should succeed without any failures.

## Scenario 1 - EC2 instance with public IP, unencrypted disk and open security group

### Scan vulnerable EC2 template
1. From this project download the **ec2.yaml**
2. Navigate to you CodeCommit Repository in AWS console
3. Select 'Add file' then "Upload file", choose the  **ec2.yaml** file you just downloaded and click "Commit changes"
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
7. Navigate to EC2 console and validate that the instance has been successfully provisioned

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
7. Navigate to S3 console and validate that the instance has been successfully provisioned

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

### Use CodePipeline to deploy resources in Scenario 1-4
