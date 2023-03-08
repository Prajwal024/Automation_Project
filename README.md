# Automation_Project
Problem Statement - 1
You have recently joined a new company named Dogecoin, which is growing very fast. As a DevOps engineer, you are required to perform  a set of tasks.  

 

Task 1: Set Up Necessary Infrastructure
Create an IAM role, a security group, an S3 bucket and launch an EC2 instance.
 

Details of the task:

 

Firstly, create an IAM role that can be attached to an EC2 instance, and attach a policy ‘AmazonS3FullAccess’. Use the role name as ‘YourFirstName_CourseAssignment’ and tag (key, value) as (EC2, S3Full).

 

Next, create a security group with the name ‘SG_YourFirstName’ and description as ‘Required for Course Assignment’ which opens inbound connections to ports 80, 443 and 22. Put the name tag as ‘WebServer Sec’ on the security group.

 

Now, set up an EC2 instance in the North Virginia (us-east-1) region:

Select AMI as ‘Ubuntu Server 18.04 LTS (HVM)’ and instance type as ‘t2.micro’.
This EC2 instance has to be created in default VPC.
Attach the IAM role which was created previously.
Create a tag with the key ‘Name’ and value ‘Web Server’ for the EC2 instance.
Attach the security group to the EC2 instance and launch it.
Finally, create an S3 bucket with the name ‘upgrad-<YourFName>’ or anything if not available.
  
  Problem Statement (Task 2)
Subtask Details
You have been assigned to write an automation bash script named ‘automation.sh’ to perform the following subtasks. 

(Hint: Running ./automation.sh with root privileges must do the following listed tasks.)

 

Your Script Should 
Perform an update of the package details and the package list at the start of the script.
sudo apt update -y
 

Install the apache2 package if it is not already installed. (The dpkg and apt commands are used to check the installation of the packages.)
Ensure that the apache2 service is running. 
Ensure that the apache2 service is enabled. (The systemctl or service commands are used to check if the services are enabled and running. Enabling apache2 as a service ensures that it runs as soon as our machine reboots. It is a daemon process that runs in the background.)
Create a tar archive of apache2 access logs and error logs that are present in the /var/log/apache2/ directory and place the tar into the /tmp/ directory. Create a tar of only the .log files (for example access.log) and not any other file type (For example: .zip and .tar) that are already present in the /var/log/apache2/ directory. The name of tar archive should have following format:  <your _name>-httpd-logs-<timestamp>.tar. For example: Ritik-httpd-logs-01212021-101010.tar                                                             Hint : use timestamp=$(date '+%d%m%Y-%H%M%S') )
The script should run the AWS CLI command and copy the archive to the s3 bucket. 
#Hint : use timestamp=$(date '+%d%m%Y-%H%M%S') ) to name  the  tar
aws s3 \
cp /tmp/${myname}-httpd-logs-${timestamp}.tar \
s3://${s3_bucket}/${myname}-httpd-logs-${timestamp}.tar
 

Copying to the S3 bucket will require AWS Command Line Interface (CLI)  to be installed in the system. You can install AWS CLI manually before writing and testing the script. 

# Installing awscli 
sudo apt update
sudo apt install awscli
 

Important:

Your script must have a variable with a value as the name of your s3-bucket at the start. Use that variable at all places where the s3 bucket is to be referred to in the script. In the same manner, your script must initialise a variable with your name. 
This project involves learning new concepts. So, you are encouraged to Google the commands and try out different things such as learn about cron job, services and commands like systemctl, dpkg and apt.
Place your script in '/root/Automation_Project/' directory.
Always run scripts with root privileges or first switch to the root user with sudo su and then run the scripts.
#Make the script executible
chmod  +x  /root/Automation_Project/automation.sh
#switch to root user with sudo su
sudo  su
./root/Automation_Project/automation.sh

# or run with sudo privileges
sudo ./root/Automation_Project/automation.sh
  
  Problem Statement (Task 3)
(Update Automation.sh and ensure it achieves the following)

1. Bookkeeping
Ensure that your script checks for the presence of the inventory.html file in /var/www/html/; if not found, creates it. This file will essentially serve as a web page to get the metadata of the archived logs. (Hitting ip/inventory.html will show the bookkeeping data)

 

At any point in time, the first line in the inventory.html file should be a header that will look like this:

cat /var/www/html/inventory.html
 

Log Type         Time Created         Type        Size


If an inventory file already exists, the content of the file should not be deleted or overwritten. New content should be only appended in a new line.


When your script runs, it should create a new entry in the inventory.html file about the following: 

What log type is archived?
Date when the logs were archived 
The type of archive
The size of the archive
Your inventory file should look like the following after multiple runs:

cat /var/www/html/inventory.html

Log Type               Date Created               Type      Size 
httpd-logs        010120201-100510         tar        10K
httpd-logs        020120201-100510         tar        40K
httpd-logs        030120201-100510        tar        4K
httpd-logs        040120201-100510        tar        6K


Hint: Ensure that your columns are tab-separated (one or more tabs).

 

2. Cron Job
Your script should create a cron job file in /etc/cron.d/ with the name 'automation' that runs the script /root/<git repository name>/automation.sh every day via the root user.

 

The script should be placed in the /root/<git repository name>/ directory. (Example: If your Git repository is named ‘Automation_Project’, the cron job will then run the script present in /root/Automation_Project/automation.sh)

 

Your automation script is supposed to check if a cron job is scheduled or not; if not, then it should schedule a cron job by creating a cron file in the /etc/cron.d/ folder.

 

Update Git Repository
Commit this script into your 'Dev' branch and merge it into the master (or main)  branch of your GitHub repository.
Finally, create a tag ‘Automation-v0.2’ after merging your 'Dev' branch to the master branch via a pull request. 
Note: "Plagiarism is strictly not acceptable. The submissions go through a plagiarism check after receiving them from all the learners. Even if your submission is caught under plagiarism after the scores are published, the submission score will be reduced to 0."

References:
https://crontab.guru/
Crontab 
Cron job: 

There are many ways to schedule a cron job. We encourage you to create a cron job file in the /etc/cron.d/ folder with the content as commands required to run the cron for the /root/<repository name>/automation.sh file via root user.

 

Hint:

The content of a cron file that executes script /root/Automation_Project/automation.sh with root privileges at an interval of one minute looks similar to this: 

cat /etc/cron.d/automation


#output
* * * * * root /root/Automation_Project/automation.sh
