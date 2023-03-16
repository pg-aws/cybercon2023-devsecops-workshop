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

## Workshop Instructions
Welcome to "Integrating Security and Compliance in Infrastructure-as-Code" workshop!
In this workshop you will learn to use CodePipeline and Checkov to detect and remediate security issues or misconfigurations in your infrastructure code.

## Lab Envrionment Setup
From this project download the **lambda.zip** and import to Lambda. The lambda function will be used with the DevSecOps pipeline to export the findings to SecurityHub.

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
7. Navigate to S3 console and validate that the instance has been successfully provisioned

## Scenario 4 - Custom policies

