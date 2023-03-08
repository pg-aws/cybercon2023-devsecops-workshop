# CyberCon 2023 - Integrating Security and Compliance in Infrastructure-as-Code Workshop 

# Table of Contents

1. [Workshop Instructions](#workshop-instructions)
2. [Lab Envrionment Setup](#lab-envrionment-setup)
3. [Lab Envrionment Setup](#lab-envrionment-setup)
4. [Lab Envrionment Setup](#lab-envrionment-setup)
5. [Lab Envrionment Setup](#lab-envrionment-setup)

## Workshop Instructions
Welcome to "Integrating Security and Compliance in Infrastructure-as-Code" workshop!
In this workshop you will learn xxx

## Lab Envrionment Setup


## Initialise Lab


## Check Pipeline


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
