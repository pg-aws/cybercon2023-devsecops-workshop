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

## Prerequisite
- You will need Administrator access to the AWS account you use in this workshop
- You will deploy all resources in **ap-southeast-2** region unless specified otherwise.
- Enable AWS SecurityHub in your account. This service will be used in our DevSecOps pipeline.

## Initialise Lab
Let’s deploy the DevSecOps pipeline using the CloudFormation template provided in this project. 

From the "environment" folder download the **devsecops-pipeline.yaml**.

Navigate to the CloudFormation console, click on "Create Stack". Complete mandatory details and submit the template.
![image](https://user-images.githubusercontent.com/126644393/225842060-c5863e32-9af4-42c2-ae96-b263fec190c1.png)

Once the CloudFormation template is deployed successfully, you will see the DevSecOps pipeline under AWS CodePipeline. 

Below is the screenshot of the pipeline after a successful execution. Please note, you may see a failed status because you haven’t initialised your CodeCommit repository yet. 

Before initiating the pipeline, let's first complete the steps in the [Check Pipeline](#Check-Pipeline) section to upload a sample CloudFormation script to your CodeCommit repository, so the pipeline will have something to test and deploy.

![image](https://user-images.githubusercontent.com/126644393/225551274-6661ae1f-7826-45f1-9188-50df5e54ac49.png)

## Check Pipeline

Let’s upload a sample CloudFormation script to CodeCommit to test the CodePipeline.

1. From the "environment" folder download the **sample-template.yaml** and 
2. Go to CodeCommit
3. Select 'Add file' then "Upload file", choose the  **sample-template.yaml** file you just downloaded and click "Commit changes"
![image](https://user-images.githubusercontent.com/126644393/225547267-a4a946f0-7535-4df0-9ded-fe2b95e22ca1.png)

4. This will automatically trigger the pipeline and go through the vulnerability scanning stages. Go to your CodePipeline to check the running status of your pipeline.
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
2. Navigate to your CodeCommit Repository in AWS console
3. Select the **sample-template.yaml** file then click 'Edit'
4. Override the original content with the **ec2.yaml** file you just downloaded and click "Commit changes"
![image](https://user-images.githubusercontent.com/126644393/225560291-5d6fccbd-2cb4-4a50-a686-e6f4b7a9241b.png)

4. Wait for CodePipeline to run and detect vulnerabilities.
5. Once completed, check the completion status of the 'CheckovTestAction' box. It should result in a 'Failed' state.
6. Click on "View in CodeBuild" to see the details of the vulnerabilities
<img alt="image" src="https://user-images.githubusercontent.com/126644393/225561838-7210a167-e127-484f-8dda-ea01d8a0b9e7.png">
7. The failed checks are: <br />
- Check: CKV_AWS_3: "Ensure all data stored in the EBS is securely encrypted" <br />
- Check: CKV_AWS_88: "EC2 instance should not have public IP."<br />
- Check: CKV_AWS_260: "Ensure no security groups allow ingress from 0.0.0.0:0 to port 80"<br />
- Check: CKV_AWS_24: "Ensure no security groups allow ingress from 0.0.0.0:0 to port 22"<br />
8. Next we will fix these issues in our CloudFormation template and upload the fixed version to CodeCommit Repository

### Fix vulnerabilities in EC2 template
1. Navigate to your CodeCommit Repository in AWS console
2. Select the **sample-template.yaml** file then click 'Edit'
3. Override the original content with the **ec2-fixed.yaml** file you just downloaded and click "Commit changes"
4. Wait for CodePipeline to run and detect vulnerabilities. 
5. Once completed, check the completion status of the 'CheckovTestAction' box. It should result in a 'Succeeded' state
6. Click on "View in CodeBuild" to see the details of the scan results


## Scenario 2 - S3 bucket with public access and incoming traffic from any IP address

### Scan vulnerable S3 template
1. Download the files from the "scenario-2" folder
2. Navigate to your CodeCommit Repository in AWS console
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
1. Navigate to your CodeCommit Repository in AWS console
2. Select the **sample-template.yaml** file then click 'Edit'
3. Override the original content with the **s3-fixed.yaml** file you just downloaded and click "Commit changes"
4. Wait for CodePipeline to run and detect vulnerabilities. 
5. Once completed, check the completion status of the 'CheckovTestAction' box. It should result in a 'Succeeded' state.
6. Click on "View in CodeBuild" to see the details of the scan results


## Scenario 3 - Essential 8 control violations
In this scenario, we will scan a CloudFormation template that violates 2 coontrols from Essential 8 framework. The CloudFormation template will deploy a DynamoDB database without KMS encryption and backup.

### Scan vulnerable DynamoDB template
1. Download the files from the "scenario-3" folder
2. Navigate to your CodeCommit Repository in AWS console
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
1. Navigate to your CodeCommit Repository in AWS console
2. Select the **dynamoDB.yaml** file then click 'Edit'
3. Override the original content with the **dynamoDB-fixed.yaml** file you just downloaded and click "Commit changes"
4. Wait for CodePipeline to run and detect vulnerabilities.
5. Once completed, check the completion status of the 'CheckovTestAction' box. It should result in a 'Succeeded' state.
6. Click on "View in CodeBuild" to see the details of the scan results


## Scenario 4 - Custom policies
Checkov comes with a set of prefined policies ([see link](https://www.checkov.io/5.Policy%20Index/cloudformation.html)) for CloudFormation templates.
In addition, you can also create your own custom policies to monitor and enforce cloud infrastructure configuration in accordance with your organization’s specific needs. For example, for certain resource types, you may want to enforce a tagging methodology or a special secure password policy; or you may want to restrict usage of a new service depending on the types of other services it is connected to.

In this scenario, we will create a custom policy to ensure AppSnync is protected by WAF and see how misconfigurations is detected.

Check out [Checkov Documentation](https://www.checkov.io/3.Custom%20Policies/Custom%20Policies%20Overview.html) to see how to write your own custom policies.

### Setup custom policy
1. Download the files from the "scenario-4" folder
2. Navigate to your CodeCommit Repository in AWS console
3. Select 'Add file' then "Create file", 
![image](https://user-images.githubusercontent.com/126644393/225547267-a4a946f0-7535-4df0-9ded-fe2b95e22ca1.png)
4. Leave the file content empty and enter "**custom-policies/\_\_init\_\_.py**" as the File name
![image](https://user-images.githubusercontent.com/126644393/225572566-b338c2a0-d877-4e50-9b45-f104a3228c74.png)
5. Fill in Author name and Email address, then click "Commit changes"
6. This will create a "__init__.py" file under a "custom-policies" directory. Next, let's follow the same steps to create the custom policy under the same directory. 
7. Navigate to the "custm-policies" folder you just created
8. Upload **AppSyncProtectedByWAF.yaml** file
9. From the left panel, expand the "Build" section, click "Build Projects", then select the "checkov-project" from the main screen
10. From the top right of the Build Project screen, click on the "Edit" dropdown then "Buildspec"
![image](https://user-images.githubusercontent.com/126644393/225575452-8964be7d-9cfc-4749-a0ee-37f345b8c3ae.png)
11. Replace Line 11 with: 
```- checkov -d . --external-checks-dir=custom-policies```
![image](https://user-images.githubusercontent.com/126644393/225576359-ff0f00ab-b7b7-4490-beb5-a10c389a6fa0.png)
12. Click "Update buildspec"
13. We are now ready to scan CloudFormation scripts with the new custom policy!

### Scan vulnerable AppSync template
1. Navigate to your CodeCommit Repository in AWS console
2. Select the **sample-template.yaml** file then click 'Edit'
3. Override the original content with the **appsyncapi.yaml** file you just downloaded and click "Commit changes"
4. Upload the static website files to the responsitory
5. Wait for CodePipeline to run and detect vulnerabilities.
6. Once completed, check the completion status of the 'CheckovTestAction' box. It should result in a 'Failed' state
7. Click on "View in CodeBuild" to see the details of the vulnerabilities
8. The scan result now includes the failed check we just created: 
![image](https://user-images.githubusercontent.com/126644393/225578385-0f7a889b-9e69-45dc-a140-bc2169b08a25.png)

9. Next we will fix these issues in our CloudFormation template and upload the fixed version to CodeCommit Repository

### Fix vulnerabilities in AppSync template
1. Navigate to your CodeCommit Repository in AWS console
2. Select the **sample-template.yaml** file then click 'Edit'
3. Override the original content with the **appsyncapi-fixed.yaml** file you just downloaded  and click "Commit changes"
4. Wait for CodePipeline to run and detect vulnerabilities. 
5. Once completed, check the completion status of the 'CheckovTestAction' box. It should result in a 'Succeeded' state.
6. Click on "View in CodeBuild" to see the details of the scan results


## Additional Exercises

### Import vulnerabilities to Security Hub
1. Download the files from the "additional-exercises" folder
2. Navigate to Lambda console and create a new function from scratch using below details:
- Function name: ImportVulToSecurityHub
- Runtime: Python 3.9
- Execution role: Create a new role with basic Lambda permissions
- Upload **ImportVulToSecurityHub.zip** to the Lambda function you just created
![image](https://user-images.githubusercontent.com/126644393/225844480-1d3369a0-48ca-4c2a-ac1e-0fc4ee641d5a.png)

Move the python files to the root folder. The file structure should look like the screenshot below: <br />
![image](https://user-images.githubusercontent.com/126644393/225810501-8b1ea8e7-5818-4b17-a029-e7fc5fa9d3fc.png)

Click on "Deploy" to apply the changes. <br />
![image](https://user-images.githubusercontent.com/126644393/225844772-f9376d4a-7cf6-404b-9e64-48eeb83fc4fa.png)

3. Once created, go to "Code" tab, scroll down to the “Runtime settings” section and click “Edit”
- Update Handler to: '''import_findings_security_hub.lambda_handler'''
![image](https://user-images.githubusercontent.com/126644393/225796690-a03e7229-9903-4dfb-b09f-ec1f908b1d91.png)

Go to "Configuration" tab, select "Permissions" from the left panel and click on the Execution role name
![image](https://user-images.githubusercontent.com/126644393/225792489-968e8ab4-8561-44d3-bb3e-5005df64fa28.png)

4. Add permissions **AmazonS3FullAccess** and **AWSSecurityHubFullAccess** to the role
![image](https://user-images.githubusercontent.com/126644393/225792866-8354ed32-5ec7-42de-97d3-a3c92dd71a64.png)

5. Upload the **buildspec-checkov.yml** file to the root of your CodeCommit Repository
6. Navigate to your CodePipeline in AWS console
7. From the left panel, expand the "Build" section, click "Build Projects", then select the "checkov-project" from the main screen
8. From the top right of the Build Project screen, click on the "Edit" dropdown then "Buildspec"
![image](https://user-images.githubusercontent.com/126644393/225575452-8964be7d-9cfc-4749-a0ee-37f345b8c3ae.png)
9. Select "Use a buildspec file" and enter "buildspec-checkov.yml" as Buildspec name
![image](https://user-images.githubusercontent.com/126644393/225590668-0711b989-2b7d-46d0-a3f2-ab9bdbcc758d.png)
10. Create a S3 bucket in your current region called "**logging-bucket-[Your Account ID]**" with ACLs enabled
![image](https://user-images.githubusercontent.com/126644393/225845604-029d392e-264f-4513-a078-7b3fa3c00ddb.png)

12. Now when the CodePipeline runs again, you will be able to view all Findings (if there are any) in Security Hub!
![image](https://user-images.githubusercontent.com/126644393/225795096-1e4603cf-f089-4fb4-b678-8735debb38ad.png)

To test this out:
1. Navigate to your CodeCommit Repository in AWS console
2. Select the **sample-template.yaml** file then click 'Edit'
3. Override the original content with the **s3.yaml** file you downloaded previously and click "Commit changes"
4. Wait for CodePipeline to complete. 
5. Once completed, navigate to Security Hub console, select 'Findings' from the left panel and check out the details of the Checkov Findings.
  
### Use CodePipeline to deploy resources in Scenario 1-3

#### Update CodePipeline
1. Navigate to your CodePipeline in AWS console
2. Click "Edit", then add a new stage after **Test** stage called "Deploy"
3. create an Action Group
4. Add an action in the Deploy stage using below details:
  - Action name: CreateStack
  - Action provider: AWS CloudFormation
  - Region: Asia Pacific (Sydney)
  - Input artifacts: SourceArtifact
  - Action mode: Create or update a stack
  - Stack name: [enter a stack name that's unique in your CloudFormation console]
  - Actifact name: SourceArtifact
  - File name: sample-template.yaml
  - Role name: arn:aws:iam::549319014430:role/DevSecOps_CloudFormation_ServiceRole
  
![image](https://user-images.githubusercontent.com/126644393/225835520-17daa992-eed2-4a6a-9b3a-1bbfc1716058.png)
![image](https://user-images.githubusercontent.com/126644393/225835587-9d4c32af-90ab-4cc5-9dba-429192707d68.png)

4. Click "Done" and Save the pipeline
  
### Scenario 1
1. Download the files from the "scenario-1" folder
2. Navigate to your CodeCommit Repository in AWS console
3. Select the **sample-template.yaml** file then click 'Edit'
4. Override the original content with the **ec2-fixed.yaml** file you just downloaded and click "Commit changes"
5. Wait for CodePipeline to complete. This will take around 6 minutes.
6. Once completed, navigate to the EC2 console and validate the EC2 instance created by CodePipeline
  
### Scenario 2
1. Download the files from the "scenario-2" folder 
2. Create a S3 bucket in **us-east-1** region called "waf-logging-bucket-[your Account ID]"
3. Switch to **us-east-1** region, and deploy the **aws-waf-security-automations.template** in CloudFormation console. 
4. In "Stack Details" page, under "Application Access Log Bucket Name", enter "waf-logging-bucket-[your Account ID]"
![image](https://user-images.githubusercontent.com/126644393/225849775-54cd23a6-002c-4a12-bdbb-0df1ca01eba9.png)

5. This CloudFormation template will take around 6 minute to complete. It will install a WAF that can be used to protect cloud resources created in Scenario 2 and 4. Once completed, note down the ARN of the WAF from the Outputs tab.
 ![image](https://user-images.githubusercontent.com/126644393/225854359-46f03ada-d6c6-4b87-b0b5-d8009d9d169f.png)

6. Open **s3-fixed.yaml**, replace Line 91 with ARN of the WAF you just created
  ```
  WebACLId: "arn:aws:wafv2:us-east-1:${AWS::AccountId}:global/webacl/waf-security-automation/abcdef-1234-5678-abcd-efghijkixxx"
  ```
7. Switch back to **ap-southeast-2** region and navigate to your CodeCommit Repository in AWS console
8. Select the **sample-template.yaml** file then click 'Edit'
9. Override the original content with the **s3-fixed.yaml** file you just updated and click "Commit changes"
10. Wait for CodePipeline to complete. This will take around 8 minutes.
11. Once completed, navigate to the S3 bucket created by the CodePipeline, upload the "**index.html**" file you just downloaded to the S3 bucket
12. Go to CloudFront, click on the Distribution created by CodePipeline, you should be able to access the webpage using the Distribution domain name
<img alt="image" src="https://user-images.githubusercontent.com/126644393/225596138-c4335c84-68c3-4e41-af20-b1a0053441f5.png">


### Scenario 3
1. Download the files from the "scenario-3" folder 
2. Navigate to your CodeCommit Repository in AWS console
3. Select the **sample-template.yaml** file then click 'Edit'
4. Override the original content with the **dynamodb-fixed.yaml** file you just downloaded and click "Commit changes"
5. Wait for CodePipeline to complete. This will take around 8 minutes.
6. Once completed, navigate to the DynamoDB console and validate the database table created by CodePipeline

# Conclusion

In this workshop, we built a DevSecOps CI/CD pipeline to scan and deploy insfracture code.

We integrated an open-source scanning tool called Checkov to perform IaC SAST analysis.

We also learnt how AWS Security Hub can be helpful in aggregating all the vulnerability findings from the scanning tool(s). 

Hope you enjoyed the workshop!
