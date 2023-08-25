# Purpose:
To familiarize myself with setting up a Jenkins server on an EC2 and manually deploying to AWS Elastic Beanstalk. In past deployments, we connected to a pre-configured Jenkins server.

## Steps to Production
### 1. Provision a Jenkins Server
#### a. Goal
Create Jenkins server in our EC2 instance to execute our pipeline steps/ 
#### b. Optimization
I created a bash script that updated and upgraded the Ubuntu repository and software and installed Jenkins and python3.10-venv. The latter created a virtual environment with its dependencies to run the application code.  
#### c. Issues
I encountered an issue installing the 'Pipeline Utility Steps' via the command line. I resolved the problem by installing the plugin on the Jenkins GUI. This plugin allows Jenkins to interact with the application files, in this case, compressing them.

### 2. Use Jenkins to Build, Test, and Zip Files
#### a. Goal
Using Jenkins allows me to automate the CI pipeline, specifically the build, test, and zipping file stages, so we wouldn’t have to waste resources and time manually building and testing the application and zipping the files separately and across different environments
#### b. Console Output
The build stage downloaded the packages specified in the requirements file necessary to build the application. Then, a test ran, and the results were saved to a file titled results. The ‘packaging output files’ stage created a zipped file here: /var/lib/jenkins/workspace/Deployment2/build/1.0.0.1.zip - remember to press continue 

### 3. Unzipping the File
#### a. Process
I navigated to the EC2 hosting the Jenkins server and went to that destination. 
I installed unzip on the EC2 and then unzipped the file with the command unzip.
#### b. Contents
The contents were the original application files from the GitHub repository, the results file from the test, a cache folder, which I believe has a copy of the code in a language understandable by the machine or program, and the dependencies necessary to build and test the code.
#### c. Optimization
Since the Jenkins pipeline zipped the application files, you could pass it to the AWS Elastic Beanstalk to deploy the application. The additional files Jenkins created would not matter because they are likely not called by any of the code. 

### 4. Manually Deploy to AWS Elastic Beanstalk
#### a. Creating IAM Roles
I created IAM roles to allow Elastic Beanstalk to set up the necessary application to run the application and to allow the EC2 environment that runs the application to use other AWS resources
#### b. Configuring the Environment
Similar to past deployments, we used the created IAM role to allow EB to provision the infrastructure. I also selected the default VPC and an availability zone. I also configured the storage to be a General SSD with a volume of 10GB.

## Successful Deployment
<img width="1118" alt="Deployment 2 Success" src="https://github.com/nalDaniels/Deployment2/assets/135375665/ae471aaf-43eb-492b-999b-88b300750c27">

## Resources
Attached here please find my CI/CD Pipeline for this deployment: https://github.com/nalDaniels/Deployment2/blob/main/CICDPipeline.md
