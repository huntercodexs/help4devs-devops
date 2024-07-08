# AWS HELPER
A simple instructions to help developers in overall cases to make configurations int the AWS Services

<br /><br />

# SUMMARY

- <a href="#IAM">IAM</a>
  - <a href="#Create-a-user-step-by-step">Create a user step by step</a>
- <a href="#BILLING">BILLING</a>
  - <a href="#Manage-Billing-Alarm">Manage Billing Alarm</a>
- <a href="#CLOUD-SHELL">CLOUD-SHELL</a>
- <a href="#AWS-CLI">AWS-CLI</a>
- <a href="#EC2 ">EC2 </a>
  - <a href="#Create">Create</a> 
  - <a href="#Destroy-Instance-EC2">Destroy Instance EC2</a>
- <a href="#AMI-AMAZON-MACHINE-IMAGE">AMI - AMAZON MACHINE IMAGE</a>
- <a href="#S3">S3</a>
  - <a href="#Create-a-default-bucket-to-storage-files">Create a default bucket to storage files</a> 
  - <a href="#Testing-bucket-access">Testing bucket access</a>
  - <a href="#Hosting-Static-Website-Using-ACLs">Hosting Static Website - Using ACLs</a> 
  - <a href="#Hosting-a-website-Using-Bucket-Policy">Hosting a website - Using Bucket Policy</a> 
  - <a href="#Hosting-a-website-Using-friendly-name">Hosting a website - Using friendly name</a>
- <a href="#ROUTE-53">ROUTE 53</a>
  - <a href="#Create-Redirect-to-EC2-Instance">Create Redirect to EC2 Instance</a>
  - <a href="#Create-a-Redirect-to-S3-AWS-Resource">Create a Redirect to S3 AWS Resource</a>
- <a href="#CLOUD-FRONT">CLOUD FRONT</a>
  - <a href="#Create-distribution">Create distribution</a>
  - <a href="#Testing-Distribution">Testing Distribution</a>
  - <a href="#Create-a-new-record-in-the-Route-53">Create a new record in the Route-53</a>
- <a href="#SQS">SQS</a>
  - <a href="#Create-a-queue">Create a queue</a>
  - <a href="#Practical-Example-using-Lambda">Practical Example using Lambda</a>
  - <a href="#Practical-Example-Using-Java">Practical Example Using Java</a>
- <a href="#ECS">ECS</a>
- <a href="#EKS">EKS</a>
- <a href="#RDS">RDS</a>
- <a href="#DYNAMODB">DYNAMODB</a>
- <a href="#API-GATEWAY">API-GATEWAY</a>
- <a href="#LAMBDA-FUNCTIONS">LAMBDA FUNCTIONS</a>
- <a href="#CLOUD-WATCH">CLOUD WATCH</a>
- <a href="#LOAD-BALANCER">LOAD BALANCER</a>
- <a href="#ACM">ACM</a>
- <a href="#VPC">VPC</a>
- <a href="#ACL">ACL</a>
- <a href="#SECURITY-GROUP">SECURITY GROUP</a>
- <a href="#COSTS-ESTIMATES">COSTS ESTIMATES</a>
- <a href="#AWS-NETWORKING">AWS NETWORKING</a>
- <a href="#S3-ROUTE53-ACM-CLOUD-FRONT">S3 ROUTE53 ACM CLOUD FRONT</a>

<br /><br />
<a href="#AWS-HELPER"><img src="midias/images/top.png" width="60" height="30" /></a>

## IAM

###### Create a user step by step

- Login in the AWS Console with the administrator account
    - https://aws.amazon.com/
- Go to IAM control panel:
    - https://us-east-1.console.aws.amazon.com/iam/home?region=us-east-2
- Click on the Item Menu on the left side named Users
    - Users
- Specify user details
    - User name
    - Custom password
- Click on Next button
    - Mark Attach policies directly
        - Choose the related permissions
            - AdministrativeAccess (for example)
- Click on Next
- Click on Create user
- Click on Return to users list

Now you will probably visualize the Users list with the last one that was created in this step by step.
So make a logout from the current account and try login using the new user that was created in the steps ago.

<br /><br />
<a href="#AWS-HELPER"><img src="midias/images/top.png" width="60" height="30" /></a>

## BILLING

###### Manage Billing Alarm

This setup is important to have a better control in the account costs and avoid surprises in the bills. For that click
on the link https://us-east-1.console.aws.amazon.com/billing/home#/preferences and check the following settings.

- Click on the [Edit] button - Alert preferences
- Mark the checkbox
    - Receive AWS Free Tier alerts
    - Receive CloudWatch billing alerts
- Click on Update

Now go to cloud watching and make the configurations as showed below

> Cloud Watch: https://console.aws.amazon.com/cloudwatch/home

- Click on the link Create alarms
- Click on Create alarm (button)
- Select Region: US East (N. Virginia)us-east-1
- Select metric
- Click on Billing link
- Click on Total Estimated Charge
- Mark the checkbox (Currency: USD)
- Click on Select metric (in the bottom right screen)
- Fill the form
- Choose Next button
- Select Notification Box
    - Fill the form to create a new Topic and inform the email to receive the alarm
- Click on Next
- Give Alarm name
- Click on Next
- Review the Configurations
- Finally, click Create alarm button

> IMPORTANT: Check your email address to Confirm "SNS - Simple Notification Service" and active this resource

<br /><br />
<a href="#AWS-HELPER"><img src="midias/images/top.png" width="60" height="30" /></a>

## CLOUD-SHELL

> ABOUT: Access the terminal commands in the AWS Cloud

It is pretty simple and doesn't need more details, just click in the Cloud Shell resource and begin the
administration using a terminal in the browser.

<br /><br />
<a href="#AWS-HELPER"><img src="midias/images/top.png" width="60" height="30" /></a>

## AWS-CLI

> NOTE: CLI means Command Line Interface

> To install and configure the amazon aws-cli follow the instructions in the follow page from AWS DOCS
https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html

[Linux Installing]

<pre>
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
</pre>

[Linux Updating]

<pre>
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install --bin-dir /usr/local/bin --install-dir /usr/local/aws-cli --update
</pre>

[Check Aws Version]

<pre>
aws --version
</pre>

[AWS CLI Configuration]

First of all you need to create the AccessKey to configure the aws-cli, then follow the steps below

- Click on the Menu in the top right side of the screen
- Choose Security credentials
- Scroll down "Access key"
    - Click on Create access key button link
    - Choose the Command Line Interface (CLI)
        - Mark the checkbox
    - Click Next
    - Fill the questions in the form ahead
        - Save the generated Access Key and also the Secret access key
    - Click on Done button

- Execute the aws configuration steps

<pre>
aws configure
AWS Access Key ID [None]: AKIA5FTZ........
AWS Secret Access Key [None]: /N+htYqjH9cf68+1...................
Default region name [None]: us-east-1
Default output format [None]: json
</pre>

Now it's possible to execute all commands available for AWS Services, for example:

<pre>
aws s3 ls
aws ec2 describe-instances
</pre>

To get more details see the https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html

<br /><br />
<a href="#AWS-HELPER"><img src="midias/images/top.png" width="60" height="30" /></a>

## EC2

###### Create

To create an EC2 instance go to EC2 Dashboard and click on the item "Instance" placed at the Menu on the left side 
of the screen, so you will fell down in EC2 Instances manager. 

Now you can follow the steps below to create a new instance.

- Click on "Launch instances" button on the top right of the screen
- Fill the form with the settings like below

<pre>
[Name and tags]

Name: your-instance-ec2-name (temporary-test-ec2)

[Application and OS Images (Amazon Machine Image)]

Select the instance image: Amazon Linux, macOS, Ubuntu, Windows...
Choose the AMI: Amazon Linux 2023
Choose the architecture: 64-bit (x86)

[Instance type]

Choose instance type: t2.micro (for Free tier eligible)

[Key pair]

Create a key pair to SSH connections (don't forget to download and save it)
Click Create key pair (temporary-test-ec2)

[Network settings]

Check VPC
Check Subnet
Check Public IP
Choose the Firewall (security groups)
  Create security group
    Allow SSH traffic from [Anywhere: 0.0.0.0/0] (just for tests)
  Select existing security group
    default or any other one available

[Configure storage]

Add volume if necessary

[Advanced details]

Check the advanced details if necessary

</pre>

Finally, click on "Launch instance" button placed on the bottom right of the screen.
Go back to the EC2 Instance list dashboard and check the instance status that was created in the steps ago.

So now get the Public IP from the instance created and connect via SSH, for example

<pre>
ssh -i "temporary-test-ec2.pem" ec2-user@ec2-54-152-4-44.compute-1.amazonaws.com
</pre>

> NOTE: Don't forget to give correct permissions for the pem file 400

In case the server is disconnecting in a short time you can change the file /etc/ssh/sshd_config to fix it

<pre>
ClientAliveInterval	300
ClientAliveCountMax	3
</pre>

###### Destroy EC2 Instance

> NOTE: It's pretty important to terminate the instance to avoid charges and bills surprised,
so for that follow the steps below

- Go to your EC2 instance
- Choose 'Instance State'
- Select 'Terminate Instance'
- Check the Instance state: Terminated

<br /><br />
<a href="#AWS-HELPER"><img src="midias/images/top.png" width="60" height="30" /></a>

## AMI - AMAZON MACHINE IMAGE

To create an Amazon AMI and use in any EC2 instance follow the steps below:

- Get Access to the target AWS account
- Go to EC2 Instances Dashboard
- Choose the EC2 to create an AMI
- Click on Actions button on the top right of the screen
- Choose Images and Models
    - Create Image or Image Create
- Review the AMI form to make proper configuration
- Choose Create Image

Now you can use this AMI to create others EC2 instances with the same configurations, installations, applications and
features, it can save a lot of time when you need to put the new services or machines online.

- Go to AMI management
- Choose the AMI
- Click right button in the AMI
- Choose Execute Instance in the AMI
- Set up the Instance Details like below

<pre>
[Name and tags]
[Ami]
[Instance type]
[Key pairs]
[Network settings]
    - VPC
    - Allow SSH traffic from: {Anywhere, My IP}
    - Configure the use of SUB-NET
    - Common security groups
[Storage management]
[Advanced Details]
    - Termination Protection ?
    - Shutdown behavior: {Stop, Terminate}
</pre>

- Click on Execute Instance button
- Check if the instance was generated in the EC2 Instances Dashboard

<br /><br />
<a href="#AWS-HELPER"><img src="midias/images/top.png" width="60" height="30" /></a>

## S3

#### Create a default bucket to storage files

S3 is a storage service that Amazon offer to storage files, also known as S3 Bucket.
So to create a bucket and connect in it to transfer files for it, follow the topics below

- Access your aws account
- Goto Amazon S3 in this link https://us-east-1.console.aws.amazon.com/s3/get-started?region=us-east-1
- Click on the link Buckets placed on left side of the screen
- Choose Create bucket and fill the information like below

<pre>
[General configuration]
AWS Region: US East (N. Virginia) us-east-1
Bucket type: General purpose
Bucket name: your-domain.com

[Object Ownership]
Select ACLs disabled (recommended)

[Block Public Access settings for this bucket]
Mark the pertinent checkbox to set up the correct access to the bucket
  Block all public access [ ]
    keep this checkbox marked unless you know what you are doing, for example: website hosting, in this case
    we are unmark this checkbox because we will test a single page in the S3 bucket

[Bucket Versioning]
Bucket Versioning: Disable

[Tags - optional (0)]
Let as is

[Default encryption]
Encryption type: Server-side encryption with Amazon S3 managed keys (SSE-S3)
  Bucket Key: Disable

[Advanced settings]
Object Lock: Disable
</pre>

- Choose Create bucket button

Now go back to the Bucket lists in the menu placed on left side of the screen to check if the bucket was created in there.

##### Testing bucket access

If everything is ok, you can upload and download files to the created bucket, and also you can use the sample Java
project to transfer data for the bucket.

This project can be found in the following repository link https://github.com/huntercodexs/help4devs/tree/aws,
you just put the access_key_id and secret_access_key in the application properties file.

<pre>
### S3 Bucket
bucket.name=your-domain.com
cloud.aws.stack.auto=false
cloud.aws.region.static=us-east-1
cloud.aws.credentials.accessKey=AKIA5FTZ.................
cloud.aws.credentials.secretKey=/N+htYqjH9cf68+1fVybO.....................
</pre>

#### Hosting Static Website - Using ACLs

> WARNING: Not recommended

You can host one static website in the S3 bucket as following below.

- Create one specific bucket to host static website:

<pre>
[General configuration]

AWS Region: US East (N. Virginia) us-east-1
Bucket Type: General purpose
Bucket name: your-domain.com

[Object Ownership]

Choose ACLs enabled
Object Ownership
  Choose Object writer
  Unmark "Block Public Access settings for this bucket"
    Mark "I acknowledge that the current settings might result in this bucket and the objects within becoming public."

[Bucket Versioning]
Let as is

Bucket Versioning: Mark Disable

[Tags - optional]
Let as is

[Default encryption]

Encryption type:
  Mark "Server-side encryption with Amazon S3 managed keys (SSE-S3)"
Bucket Key: Disable

[Advanced settings]
Object Lock: Disable
</pre>

- Choose Create bucket

Now you will be redirected automatically to bucket list and probably the bucket it will be listed in that one. 
So now you can click on the your-domain.com to visualize all options to configure this bucket.

- Choose the Upload button at middle of the screen to send the files into the bucket. 
- Select the files to upload
- After file uploaded, just click on Upload button placed at the bottom screen
- Also, you can upload one entirely folder that contains a website content
- Go to the bucket files list and choose the Properties Tab
- Scroll down until "Static website hosting" and click on Edit
- Mark "Static website hosting" as Enable
- Identify the index document
  - Index document: index.html
- Finally, click on Save changes
- Check and take note the URL created to access the static website (at the bottom of screen)
  - http://your-domain.com.s3-website-us-east-1.amazonaws.com

If you try to access the website right now, probably you will receive an HTTP error "403 Forbidden", because 
still need to make a few others changes in that bucket, so lets moving on.

- Scroll down until "Object Ownership" iIn the "Permissions Tab"
- Click on Edit
- Ensure that the "Bucket owner preferred" is marked
- Go back to "Objects Tab" select the files and folders
- Click on Actions menu
- Choose "Making public using ACL"
- Choose Make public button
- Click on OK button

Since for now, if everything was configured correctly you will be able to access the application website from 
the URL mentioned a few steps earlier, just for example:

> http://your-domain.com.s3-website-us-east-1.amazonaws.com

> NOTE: We don't have a DNS in this situation

#### Hosting a website - Using Bucket Policy

In this case we will use a complete environment to publish a static website with TLS and specific domain 
name, sSo let's get started and hands on.

> NOTE: In this case we will use the huntercodexs domain to create all configurations, so keep in your 
> mind that you should be changed this information for your current needed.

- Click on Create bucket button to create one specific bucket to host static website:
- Fill the form as follows

<pre>
[General configuration]

AWS Region: US East (N. Virginia) us-east-1
Bucket Type: General purpose
Bucket name: huntercodexs.com

[Object Ownership]

Choose "ACLs disabled (recommended)"
Block Public Access settings for this bucket
  Mark "Block all public access"

[Bucket Versioning]

Bucket Versioning:
  Mark Disable

[Tags optional]
> Let as is

[Default encryption]

Encryption type: Mark "Server-side encryption with Amazon S3 managed keys (SSE-S3)"
Bucket Key: Enable

[Advanced settings]

Object Lock: Disable
</pre>

- Choose Create bucket

Now you will be redirected automatically to the bucket list and probably the created bucket it will be 
listed in that one. So now, you can click on the huntercodexs.com to visualize all options and configure 
this bucket.

- Choose the Upload button at middle of the screen to send the files into the bucket.
- Upload files
- Click on Upload button 
- Go to S3 bucket details
- Click "Properties Tab"
  - Check if "Static website hosting" is Disabled
- Now click on "Permissions Tab"
  - Check if "Block public access (bucket settings)" is on
  - Click on Edit
  - Unmark the item "Block all public access"
    - Click on Save changes
      - Put confirm in the input text box
      - Click on Confirm button
  - Now the "Block public access (bucket settings)" is off
  - Still in the "Permissions Tab" scroll down until "Bucket policy" and click on Edit
    - Add the following policy

> IMPORTANT: Change the Resource field to your current Bucket ARN

<pre>
{
    "Version": "2012-10-17",
    "Id": "Policy1720049315656",
    "Statement": [
        {
            "Sid": "AllowPublicRead",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::huntercodexs.com/*"
        }
    ]
}
</pre>

- Scroll down at the bottom of the page and click on Save changes
- Now go to "Properties Tab" and scroll down until "Static website hosting"
- Click on Edit
  - Mark Enable to "Static website hosting"
  - In "Hosting type" mark "Host a static website"
  - Give an index document name: index.html
  - Choose Save changes
- Still in the "Properties Tab" scroll down again until "Static website hosting" and get the URL generated
  - Example: http://huntercodexs.com.s3-website-us-east-1.amazonaws.com
  - If everything is ok, so you probably will receive the index.html page in your browser, do try it.

#### Hosting a website - Using friendly name

> NOTE: This step requires an DNS previously registered and activated in any DNS service provider, for more details 
> see the section Route-53 along this documentation

Since now, we are going to create a more friendly name to our static website because the current name that should 
be something like that huntercodexs.com.s3-website-us-east-1.amazonaws.com, and it is very confusing, 
stranger and hard to understand and memorize, and for that we will use the Route-53 from AWS Services, so let's go.

- Get access into Route-53
- Check the Region and Zone (normally in this case ís a Global)
- Look up for "Hosted zones" on the left side at the screen 
- Click on "Create hosted zone" button
- Fill the form with the following information

<pre>
Domain name: huntercodexs.com
Description (optional): 
Type: Public hosted zone
Tags:
</pre>

- Click on Create hosted zone

> IMPORTANT: Don't forget to configure your DNS service provider with the NS name services available in the hosted
> zone that you just have created, this information can be grabbed in the "Hosted zone details"

- Now you will be probably redirected to huntercodexs.com hosted zone, where you will see one list of records to 
routes and DNS configurations, so for now click on Create record and follow the steps below

<pre>
Record name: (huntercodexs.com)
Record type: A - Routes traffic to an IPv4 address and some AWS resoruces
Alias: Marked
Route traffic to: 
  - Alias to S3 website endpoint
  - US East (N. Virginia)
  - s3-website-us-east-1.amazonaws.com
Routing policy: Simple routing
</pre>

- Click on Create records button
- Now, create another two record like below

<pre>
Record name: site (huntercodexs.com)
Record type: CNAME – routes traffic to another domain name and to some AWS resources
Value: huntercodexs.com
TTL(seconds): 300
Routing policy: Simple routing
</pre>

<pre>
Record name: www (www.huntercodexs.com)
Record type: CNAME – routes traffic to another domain name and to some AWS resources
Value: huntercodexs.com
TTL(seconds): 300
Routing policy: Simple routing
</pre>

- Click on Create records button

- Now, access the static website using the new name: 
  - http://huntercodexs.com
  - http://huntercodexs.com
  - http://www.huntercodexs.com

> NOTE: This can take a several times to be activated by your DNS service provider

- The image below show the Route53 configured and running

![aws-route53-hosted-zone-sample.png](midias/images/aws-route53-hosted-zone-sample.png)

<br /><br />
<a href="#AWS-HELPER"><img src="midias/images/top.png" width="60" height="30" /></a>

## ROUTE 53

#### Create Redirect to EC2 Instance

To get start the Route 53 configuration have in hand the following information

> NOTE: All information about one specific hosted zone can be obtained at section "Hosted zone details" in the 
> proper hosted zone at top of that

<pre>
    #DNS na AWS
    ns-1234.awsdns-00.org
    ns-1234.awsdns-00.co.uk
    ns-1234.awsdns-00.com
    ns-1234.awsdns-00.net

    #DNS in the domain server
    ns1.dns-parking.com
</pre>

- In the AWS manager console seek by Route 53 and click in it.

- If you have a hosted zone on Route 53, click on the "Hosted zone" link to access the options for
  zone management, and on this screen it is possible to create a new zone or edit an existing one. 

- You, too, can use the menu on the left side of the "Hosted Zones" screen to access this screen.

- Both in the previous case and in the case of being the first zone to be created, click on the "Create hosted zone" button.

- On the zone creation screen, enter the domain name, comment and type (public).

- When clicking on Create you will be directed to the created zone/domain screen, where you can carry out the
  settings necessary to activate routing for it.

- Click on Create record to be directed to the configuration screen for the record/domain to be routed.

- Fill in the registration name, choose the type of registration, in value enter the IP (or Elastic IP) of the machine
  which is in the cloud on AWS, TTL and Routing Policy, for example:

<pre>
Registration name: your-domain.com or www.your-domain.com
Record Type: A – Routes traffic to an IPv4 address and some AWS resources
Value: 54.51.92.190 (Elastic IP)
TTL: 300 (seconds)
Routing policy: Simple routing
</pre>

<pre>
Registration name: your-domino.com or www.your-domain.com
Record type: CNAME – routes traffic to another domain name and to some AWS resources
Value: your-domain.com
TTL: 300 (seconds)
Routing policy: Simple routing
</pre>

- Obtain DNS data from AWS to insert into your hosting or registry server, for example:
<pre>
ns-1234.awsdns-00.org
ns-1234.awsdns-00.co.uk
ns-1234.awsdns-00.com
ns-1234.awsdns-00.net
</pre>

- Go to your hosting or registration service, DNS management panel and enter one by one the DNS entered by AWS.

> NOTE: You may need to wait a few moments for the redirection and feature activation to take effect.

#### Create a Redirect to S3 AWS Resource

- Access the AWS Account
- Go to Route-53 service
- Look for Hosted zones
- Click on Create hosted zone button
- In the form to create a hosted zone
  - Domain name: you-domain.com
  - Description(optional): Desc....
  - Type: Public hosted zone
  - Tags: ...
  - Click on Created hosted zone
- Now go back to Hosted zone list, you will probably see the hosted zone created in that list
- Click on the hosted zone that you just have created link
- Now, if everything was made correctly, you should be able to see the NS Servers available for your account/domain, 
for example:

<pre>
ns-1234.awsdns-00.org
ns-1234.awsdns-00.co.uk
ns-1234.awsdns-00.com
ns-1234.awsdns-00.net
</pre>

- Note that the expected type for the correct record must be "NS
- Now, go to your service provider DNS in the web
- Put the DNS servers from AWS in the DNS Servers in the proper section in your current service provider
- Click on Save to activate the changes

> NOTE: To do that you need to have an account and domain registered in any register domain service, for example:
> GoDaddy, UOL, Hostinger, Alligator among others. So go to in the section DSM/Nameservers from your current
> service provider and get the DNS Servers that are affordable in there.

- Go back to Route-53 in your AWS account
- Click on the target hosted zone link to edit it
- Click on Create record button to create the first record, and follow the information below:

<pre>
Registration name: www (www.your-domain.com)
Record type: CNAME – routes traffic to another domain name and to some AWS resources
Value: http://your-domain.com
TTL: 300 (seconds)
Routing policy: Simple routing
</pre>

- Click on Save button, and that's it
- Now, let's go created a second record in the hosted zone
- Click on Create record again in the same hosted zone, and follow the information below:

<pre>
Registration name: s3 (s3.your-domain.com)
Record type: CNAME – routes traffic to another domain name and to some AWS resources
Value: s3.your-domain.com.amazon-aws-bucket
TTL: 300 (seconds)
Routing policy: Simple routing
</pre>

- Choose Create records button
- Test the access using your web browser

> NOTE: You may need to wait a few moments for the redirection and feature activation to take effect.

<br /><br />
<a href="#AWS-HELPER"><img src="midias/images/top.png" width="60" height="30" /></a>

## CLOUD FRONT

#### Create distribution

Below is how to create a sample distribution in the Cloud Front.
Follow these steps to delivery a content successfully. 

- Origin

<pre>
Origin domain: help4devs.com.s3.us-east-1.amazonaws.com
### Do not choose the "Use website endpoint"
Origin path - optional: 
Name: help4devs.com.s3.us-east-1.amazonaws.com
Origin access: Public
Add custom header - optional: 
Enable Origin Shield: No
</pre>

- Default cache behavior

<pre>
Path pattern: Default (*)
Compress objects automatically: Yes
</pre>

Viewer

<pre>
Viewer protocol policy: Redirect HTTP to HTTPS
Allowed HTTP methods: GET, HEAD
Restrict viewer access: No
</pre>

Cache key and origin requests

<pre>
Cache policy and origin request policy (recommended): Marked
Response headers policy - optional:
</pre>

- Function associations - optional

<pre>
Viewer request: No association
Viewer response: No association
Origin request: No association
Origin response: No association
</pre>

- Web Application Firewall (WAF)

<pre>
Do not enable security protections: Marked
</pre>

- Settings

<pre>
Price class: Use all edge locations (best performance)
Alternate domain name (CNAME) - optional: help4devs.com
Custom SSL certificate - optional: Custom SSL certificate - optional (17761472-a01d-422e-9fdf-5e6c14a0f558)
Security Policy: TLSv1.2_2021 (recommended)
Supported HTTP versions: HTTP/2
Default root object - optional: index.html
Standard logging: Off
IPv6: On
Description - optional: website help4devs
</pre>

#### Testing Distribution

- You can test the access using the "Distribution domain name" placed in the "General Tab" inside the current 
"Cloud Front Distributed" that you just have created, for example:

<pre>
https://dijdcwprgmu5e.cloudfront.net
</pre>

#### Create a new record in the Route-53

- In the Route-53 look up for the target hosted zone and click on it
- Now, create a new record clicking on Create record button
- Fill the form as follows

<pre>
Record name: (help4devs.com)
Record type: A - Routes traffic to an IPv4 address and some AWS resources
Alias: Marked
Route traffic to: Alias to Cloud Front distribution
Region: US East (N. Virginia)
Distribution: (help4dev.com) dijdcwprgmu5e.cloudfront.net
Routing policy: Simple routing
</pre>

- Choose Create record button
- Testing your cloud front application access via HTTP/HTTPS, for example:
  - http://help4devs.com (should be redirected to HTTPS automatically)
  - https://help4devs.com

<br /><br />
<a href="#AWS-HELPER"><img src="midias/images/top.png" width="60" height="30" /></a>

## SQS

#### Create a queue

To create a simple queue in the SQS service at Amazon AWS services follow the steps below.

- Goto SQS dashboard
- Click on Queues at the menu on the left side of the screen
- Click on Create queue button
- Fill the form as follows

<pre>
[Details]
Type: (Standard | FIFO) ## You can change it later if necessary
Name: queue-name-test (Standard) | queue-name-test.fifo (FIFO)

[Configuration]
Visibility timeout: 30 (seconds) ## This means how long time the message should be visible
Message retention period: 4 days
Delivery delay: 0 seconds
Maximum message size: 256KB
Receive message wait time: 0 seconds

[Encryption]
Server-side encryption: Enabled
Encryption key type: Amazon SQS key (SSE-SQS)

[Access policy]
Choose method: Basic
Define who can send messages to the queue: Only the queue owner
Define who can receive messages from the queue: Only the queue owner

[Redrive allow policy]
Select which source queues can use this queue as the dead-letter queue.: Disabled

[Dead-letter queue]
Set this queue to receive undeliverable messages: Disabled

[Tags - Optional]
let as is
</pre>

- Click on Create queue

Now, you probably will be redirected to queue details page, so look up for "Send and receive messages" button and 
click on it to test the message send. At this moment we have two box in the screen, Send message and Receive messages, 
so now put one text message in the input text and click on Send message button on the top right of the page.

When you click on the Send message button, you must be able to see the change in the Messages available in the 
"Receive messages" in the second box at the screen. This means that there is one message awaiting consumer to get and 
process that message.

![aws-sqs-queue-sample.png](midias/images/aws-sqs-queue-sample.png)

Once the message is captured/read by the consumer, it has 30 seconds to process the message until the message go back 
for the queue and stay available again for another consumers. You can click on the "Poll for messages" button to 
download/get the messages that are in the queue.

Below is one image that shown all this process

![aws-sqs-queue-sample-polled.png](midias/images/aws-sqs-queue-sample-polled.png)

Still talking about the queue edit functionality, it is possible to purge the queue using a button placed in the queue 
details, but in this case be carefully because this procedure will be removing all messages from the queue.

Finally, if you need to create one queue that avoid duplication message, so you need to choose the FIFO type to enable 
the deduplication functionality, below follow one simple configuration to apply in this situation.

![aws-sqs-queue-fifo-deduplication.png](midias/images/aws-sqs-queue-fifo-deduplication.png)

How can see in the above image, the Type FIFO has enabled one more option in the Configuration section, witch offer 
flags and settings to avoid duplication in the queue.

In the FIFO queues all duplicated message will be ignored automatically to avoid duplication in the process, because
the queue configuration is enabled to queue and not for group ID. 

When we need to guarantee the unique message separated by group ID, we need to make one configuration like below

![aws-sqs-queue-fifo-group-id.png](midias/images/aws-sqs-queue-fifo-group-id.png)

But it is very important to know that the message should be grouped by one "Message group ID", for example: payment, sales, stock, 
blocked, dispatch, etc...

![aws-sqs-queue-fifo-group-id-sample.png](midias/images/aws-sqs-queue-fifo-group-id-sample.png)

Above we can see four message that have the same content

<pre>
{"id": 1, "amount": 100, "message": "ok"}
</pre>

but the "Message group ID" for each one is different, we have: payment, sales, purchase and dispatch. This detail 
can be found in the message clicking on the link message and going to "Details Tab" at the same message.

![aws-sqs-queue-fifo-message-details.png](midias/images/aws-sqs-queue-fifo-message-details.png)

#### Practical Example using Lambda

#### Practical Example Using Java

<br /><br />
<a href="#AWS-HELPER"><img src="midias/images/top.png" width="60" height="30" /></a>

## ECS

<br /><br />
<a href="#AWS-HELPER"><img src="midias/images/top.png" width="60" height="30" /></a>

## EKS

<br /><br />
<a href="#AWS-HELPER"><img src="midias/images/top.png" width="60" height="30" /></a>

## RDS

<br /><br />
<a href="#AWS-HELPER"><img src="midias/images/top.png" width="60" height="30" /></a>

## DYNAMODB

<br /><br />
<a href="#AWS-HELPER"><img src="midias/images/top.png" width="60" height="30" /></a>

## API-GATEWAY

<br /><br />
<a href="#AWS-HELPER"><img src="midias/images/top.png" width="60" height="30" /></a>

## LAMBDA FUNCTIONS

#### PYTHON 

To create a python lambda function in the aws services follow the steps below.

- Get access in your aws account
- Goto Lambda functions dashboard
- Lookup for functions on the left menu of the screen
- Click on Create function button
- Fill the form as follows

<pre>
[Create function]
Choose one type of lambda function: 
  Author from scratch
  Use a blueprint
  Container image

[Basic information]

Funcation name: hello-world-lambda-function-py
Runtime: Python 3.12
Architecture: x86_64

[Permissions]
let as is

Change default execution role: Create a new role with basic Lambda permissions

[Advanced settings]
let as is
</pre>

- Click on Create function

You will be redirected for the panel of the lambda function just created, so now you must be able to see one section 
that means "Code source"

<pre>
import json

def lambda_handler(event, context):
    print "Running lambda_handler..."
</pre>

After created the function and code correctly, go further in the steps below

- Click on Deploy button
- Click on Test button

The result should be something like below

![aws-lambda-sample-python.png](midias/images/aws-lambda-sample-python.png)

In this point, you can click in the "Monitor Tab" showed in the above image and click on "View CloudWatch logs" 
to get access to the AWS CloudWatch service and see the logs generated by this simple function.

Below in an image that describe this situation

![aws-lambda-sample-python-watch-cloud-logs.png](midias/images/aws-lambda-sample-python-watch-cloud-logs.png)

If you click on some link related to logs it will be detailed the log process that this functions has been generated, 
and should be like something below

![aws-lambda-sample-python-logs-detail.png](midias/images/aws-lambda-sample-python-logs-detail.png)

It is possible to see in the above image the message produced by the current lambda function testing here

#### JAVA

To create a java lambda function in the aws services follow the steps below.

- Get access in your aws account
- Goto Lambda functions dashboard
- Lookup for functions on the left menu of the screen
- Click on Create function button
- Fill the form as follows

<pre>
[Create function]
Choose one type of lambda function: 
  Author from scratch
  Use a blueprint
  Container image

[Basic information]

Funcation name: hello-world-lambda-function-java
Runtime: Java 8+
Architecture: x86_64

[Permissions]
let as is

Change default execution role: Create a new role with basic Lambda permissions

[Advanced settings]
Enabled function URL: Marked
Auth type: NONE
Invoke mode: BUFFERED (default) 
</pre>

- Click on Create function

You will be redirected for the panel of the lambda function just created, so now you must be able to see one section
that means "Code source". In this case, when we are using Java Runtime, the IDE code is not available, so we need to 
follow another way to get everything done.

Follow the steps below to put Java application running in the lambda function.

- 


<br /><br />
<a href="#AWS-HELPER"><img src="midias/images/top.png" width="60" height="30" /></a>

## CLOUD WATCH

The Cloud Watch is a complete dashboard to give support an analysis of the events, metrics, crash, and 
others things relate to that. Below is the short explanations about the resources offered in the AWS resource.

#### Alarms
#### Logs
#### Metrics
#### X-Ray-traces
#### Events
#### Application Signals
#### Networking monitoring
#### Insights

<br /><br />
<a href="#AWS-HELPER"><img src="midias/images/top.png" width="60" height="30" /></a>

## LOAD BALANCER

<br /><br />
<a href="#AWS-HELPER"><img src="midias/images/top.png" width="60" height="30" /></a>

## ACM

<br /><br />
<a href="#AWS-HELPER"><img src="midias/images/top.png" width="60" height="30" /></a>

## VPC

<br /><br />
<a href="#AWS-HELPER"><img src="midias/images/top.png" width="60" height="30" /></a>

## ACL

<br /><br />
<a href="#AWS-HELPER"><img src="midias/images/top.png" width="60" height="30" /></a>

## SECURITY GROUP

<br /><br />
<a href="#AWS-HELPER"><img src="midias/images/top.png" width="60" height="30" /></a>

## COSTS ESTIMATES

<br /><br />
<a href="#AWS-HELPER"><img src="midias/images/top.png" width="60" height="30" /></a>

## S3 ROUTE53 ACM CLOUD-FRONT

In this topic we will host a static website in the Amazon Web Services using S3 Bucket + Route53 + ACM + CLOUD-FRONT. 
To do that, it's required that you have previously registered one domain name service in any DNS services provider 
like Godaddy, Hostinger, Hostigator, among others.

So in this, just for clarify and explain everything in a better way, we are going to use the help4devs.com domain, 
that is a product from huntercodexs.com witch is maintained by jereelton-devel (witch have written this 
documentation), so lets get started and hands on.

First of all you need to keep in your mind the following considerations:

- The name of the domain is: help4devs.com
- The name of the S3 bucket it will be the same: help4dev.com
- The hosted zone in the Route53 it will be the same as well: help4devs.com
- The distribution in the Cloud Front also should be: help4devs.com
- Finally, the Certificate should be requested for: help4devs.com and www.help4fevs.com

> IMPORTANT: It is very important to check if your application can handle the browser cache, and 
> probably you will need to clear the cache in the web browser to get the expected result.

> NOTE: The website must be a static website commonly using HTML, CSS, and JS, so ensure that 
> the project web to deploy in this scenario follows this specification.

Once it defined and well clearly those things, lets moving on for the start procedures.

#### S3 - Bucket creation

- Goto S3 Bucket in the AWS services
- Click on Create bucket button
- Fill the form as follows

<pre>
[General configuration]
AWS Region: US East (N. Virginia) us-east-1
Bucket type: General purpose
Bucket name: help4devs.com

[Object Ownership]
Select ACLs disabled (recommended)

[Block Public Access settings for this bucket]
Block all public access: Unmark
  Mark the checkbox alert to advice about public access
  [x] I acknowledge that the current settings might result in this bucket and the objects within becoming public.

[Bucket Versioning]
Bucket Versioning: Disable

[Tags - optional (0)]
Let as is

[Default encryption]
Encryption type: Server-side encryption with Amazon S3 managed keys (SSE-S3)
  Bucket Key: Disable

[Advanced settings]
Object Lock: Disable
</pre>

- Click on Create bucket

Now you will be redirected to bucket list, and you must be able to see the bucket that you just have created 
in this process, so click in the bucket created and go further with the steps.

![aws-s3-bucket-created.png](midias/images/s3-route53-acm-cloudfront/aws-s3-bucket-created.png)

- Click on the Upload button to send the static files to the bucket
- Afterward you will see the files in the "Object Tab" inside the bucket

![aws-s3-bucket-created.png](midias/images/s3-route53-acm-cloudfront/aws-s3-bucket-files-uploaded.png)

- Click on the "Properties Tab"
- Scroll down until "Static website hosting" and click on Edit
- Fill the form as follows

<pre>
[Static website hosting]

Static website hosting: Enable
Hosting type: Host a static website
Index document: index.html
Error document-optional:
Redirection rules – optional: 
</pre>

- Click on Save changes
- Scroll down again until "Static website hosting" and check if the result is something like below

![aws-s3-bucket-created.png](midias/images/s3-route53-acm-cloudfront/aws-s3-static-website-hosting.png)
See that there is one default url generated by AWS witch can be used to access your static website in the web browser

- Now, goto "Permissions Tab"
- Scroll down until Bucket policy and click on Edit button
- You can use the "AWS Policy Generator" to make it
- The rule should be something like that

<pre>
{
	"Version": "2012-10-17",
	"Id": "Policy1720231682330",
	"Statement": [
		{
			"Sid": "Stmt1720231676574",
			"Effect": "Allow",
			"Principal": "*",
			"Action": "s3:GetObject",
			"Resource": "arn:aws:s3:::help4devs.com/*"
		}
	]
}
</pre>

> IMPORTANT: replace Resource field by yor current resource "arn:aws:s3:::help4devs.com/*" and don't forget to use
> / * at the end of the string

So now you can test if the bucket is available to access via browser using the url generated by AWS in the previous step.

#### Route 53 - Hosted Zone creation

- Get access to your AWS account and goto Route53 dashboard
- Click on Hosted zone on the left menu at the screen
- Choose the Created hosted zone button
- Fill the forms as follows

<pre>
[Hosted zone configuration]

Domain name: help4devs.com
Description - optional: Website Help4Devs (Godaddy)
Type: Public hosted zone

[Tags]
let as is
</pre>

- Click on Created hosted zone

You will be redirected to hosted zone list automatically, and you should be able to see the hosted zone that was 
just created in the hosted zone list, so for now click on it according the image below

![aws-route53-hosted-zone-created.png](midias/images/s3-route53-acm-cloudfront/aws-route53-hosted-zone-created.png)

At this moment, you will see something like below

![aws-route53-hosted-zone-records.png](midias/images/s3-route53-acm-cloudfront/aws-route53-hosted-zone-records.png)

Now, you need to get the NS Servers identification and put on the correct place in your DNS service provider to 
activate the redirecting for you domain into Route 53 AWS service, this step it is so important.

- After that, you can move on for the next step, request a certificate.

#### ACM - Certificate request

Once you have created a hosted zone and one static website, follow these steps below to get the SSL certificate 
generate by Amazon Web Services.

- Go to the ACM (Amazon Certificate Manager)
- Look up for List certificates and click on it
- Now, click on Request button to start a request certificate
- Follow the instructions as follows

<pre>
[Certificate type]

Request a public certificate: Marked
Domain names: help4devs.com
### You can add more than one domain name, for example: www.help4devs.com, site.help4devs.com, and etc...

[Validation method]

DNS validation - recommended: Marked

[Key algorithm]

RSA 2048: Marked

[Tags]

Use as is
</pre>

- Click on Request button (at the bottom of the page)
- Now, you need to await for finished process to request certificate
- The List certificate dashboard should be like below

![aws-s3-acm-certificate-request.png](midias/images/s3-route53-acm-cloudfront/aws-s3-acm-certificate-request.png)

- Click on the link (guid marked in orange box in the above image) to access the certificate details
- Scroll down until Domains and click on "Create records in Route 53" as showed below

![aws-s3-acm-certificate-details.png](midias/images/s3-route53-acm-cloudfront/aws-s3-acm-certificate-details.png)

> NOTE: Also, you can see that there is one orange box to give a highlight in the domains that were 
> registered in the current SSL certificate request, help4dev.com and www.help4devs.com

- In the next screen you just have to click on Create records, this action it will create the inputs in the Route53
in the current domain help4devs.com. These inputs are related to SSL certificate and will be turning the HTTPS 
available for the current domain.
- OK, for now the ACM is successfully completed, and you just need to await  for the certificate approved by Amazon to
go further in the configuration

#### CLOUD-FRONT - Distribution creation

If all previous steps are been made successfully, you can distribute the static website using CloudFront, so for
that just follow the steps ahead and everything will work fine.

- Go to Cloud Front from AWS services
- Click on Distributions link on the menu on the left side of the screen
- Click on Create distribution button at the top right of the screen
- Fill the form as follows

<pre>
[Origin]

## When you click in that one, a options it will be shown automatically based on S3 bucket static website
Origin domain: help4devs.com.s3.us-east-1.amazonaws.com
## Don't click on "Use website endpoint", just go ahead
Origin path - optional:
Name: help4devs.com.s3.us-east-1.amazonaws.com
Origin access: Public
Add custom header - optional: 
Enable Origin Shield: No

[Default cache behavior]

Path pattern: Default (*)
Compress objects automatically: Yes

[Viewer]

Viewer protocol policy: Redirect HTTP to HTTPS
Allowed HTTP methods: GET, HEAD
Restrict viewer access: No

[Cache key and origin requests]
Cache policy and origin request policy (recommended): Marked
Response headers policy - optional: 

[Function associations - optional]
Ignore this step

[Web Application Firewall (WAF)]

Do not enable security protections: Marked

[Settings]

Price class: Use all edge locations (best performance)
Alternate domain name (CNAME) - optional: help4devs.com
Custom SSL certificate - optional: help4devs.com (17761472-a01d-422e-9fdf-5e6c14a0f558)
TLSv1.2_2021 (recommended): Marked
Supported HTTP versions: HTTP/2
Standard logging: Off
IPv6: On
</pre>

- Click on Create distribution

Now, you need to await for the complete deploying to activate the delivery, you can do it checking the status in the 
list of distribution from Cloud Front, as showed below

![aws-cloud-front-distribution.png](midias/images/s3-route53-acm-cloudfront/aws-cloud-front-distribution.png)

If the status is Enabled and Last modified is not null, so you have a sure that everything is done, so for now click 
on distribution link "E2YCFFG62C0DNC" according showed in the above image to get more details about it.

In the "General Tab" there is one box named Details with one field defined as "Distribution domain name" witch have 
a small bullet next to, so click on it and get the url, for example: https://dijdcwprgmu5e.cloudfront.net. Look that 
it is a HTTPS url, i.e. it will be request a safety connection guaranteed by Amazon Certificate Manager

![aws-cloud-front-details-domain.png](midias/images/s3-route53-acm-cloudfront/aws-cloud-front-details-domain.png)

#### Create Route to friendly URL

![aws-route53-final-records.png](midias/images/s3-route53-acm-cloudfront/aws-route53-final-records.png)

In the above image we can see a complete hosted zone configured to work correctly, there is one record of A type, and 
two records CNAME typed, these two CNAME records are related to our certificate that were created when we clicked on 
the "Create records in Route 53" previously in this topic.

Now, talking about the first one present in this hosted zone, it was created to redirect all HTTP/HTTPS requests for 
our CloudFront distribution, and this was made according the steps below

- Click on Create record
- Fill the form as showed in the image below

![aws-route53-record-domain.png](midias/images/s3-route53-acm-cloudfront/aws-route53-record-domain.png)

- Just click on Create records button to finish everything

Now you can access the static website using HTTPS and one friendly URL, for example: https://help4devs.com

<br /><br />
<a href="#AWS-HELPER"><img src="midias/images/top.png" width="60" height="30" /></a>

## AWS NETWORKING

In this topic we can see one small concept project to view how to work an environment based on AWS Cloud services.
Belo is the overview image to presentation this subject in a quickly and understandably way.

![know-how-AWS NETWORK.png](midias/images/aws-networking.png)

As we can see in the above image, there is a lot of resources used from AWS Cloud services that those are describe as
follows below:

- AWS CLOUD
- REGION
- VPC
- INTERNET GATEWAY
  - ROUTER
    - AVAILABILITY ZONE
    - PUBLIC ROUTER TABLE
      - ACL
        - PUBLIC SUBNET
          - SECURITY GROUP 1
            - MFA
              - EC2 - MAINTENANCE
          - SECURITY GROUP 2
            - EC2 - NGINX REVERSE PROXY
      - NAT INTERNET
    - PRIVATE ROUTER TABLE
      - ACL
        - PRIVATE SUBNET
          - SECURITY GROUP 3
            - EC2 - MICROSERVICE
              - QUANTITY = 4
                - SERVICE1 = hello-world-1.jar
                - SERVICE2 = hello-world-2.jar
                - SERVICE3 = hello-world-3.jar
                - SERVICE4 = hello-world-4.jar
            - DATABASE
              - RDS: MYSQL

---
<small>
Huntercodexs is powered by jereelton-devel
</small>
<br />
<br />
