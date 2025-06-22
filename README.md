# 🔄 Daily Backup to S3 Automation (AWS + Linux Cron)

This project shows how I automated a **daily backup task** using a **shell script** and **cron job** on an EC2 Linux server. The backup is stored in an **Amazon S3 bucket** using the AWS CLI.


---

## ✅ What It Does

* Compresses a folder (`~/myfiles`) into a `.tar.gz` file
* Uploads the file to an S3 bucket
* Deletes the backup file from the server after upload
* Runs automatically every day using a cron job

---

## 🔧 Tools Used

* Amazon EC2 (Amazon Linux 2023)
* IAM
* Amazon S3
* AWS CLI
* Bash (Shell script)
* Cron (Linux scheduler)

---

## 🪜 Steps

### 1. Set Up the Server

* Launched an EC2 instance on AWS (free tier)
* Connected to the EC2 instance using SSH
* Installed AWS CLI:
  sudo yum install -y awscli

* Configured AWS CLI:
  aws configure
  > Ensured my IAM user has S3 permissions. 
---

### 2. Created an S3 Bucket
aws s3 mb s3://your-backup-bucket-name
> Replace `your-backup-bucket-name` with a unique name.

---

### 3. Created the Folder to Back Up
mkdir ~/myfiles
echo "Sample file 1" > ~/myfiles/file1.txt
echo "Sample file 2" > ~/myfiles/file2.txt

---

### 4. Wrote the Backup Script
touch backup.sh
 
**File: `backup.sh`**


#!/bin/bash

TIMESTAMP=$(date +%F-%H%M)
BACKUP_FILE="backup-$TIMESTAMP.tar.gz"
SOURCE_DIR=~/myfiles

tar -czf $BACKUP_FILE $SOURCE_DIR
aws s3 cp $BACKUP_FILE s3://your-backup-bucket-name/
rm $BACKUP_FILE

echo "Backup completed at $TIMESTAMP"

-------
Make it executable:
  chmod +x backup.sh


---

### 5. Tested the Script
./backup.sh

✅ Then I checked the S3 bucket to see the uploaded file.

---

### 6. Scheduled the Automation with Cron

Edited the crontab:
crontab -e


Added this line to run the backup **daily at 1:00 AM**:

0 1 * * * /home/ec2-user/backup.sh >> /home/ec2-user/backup.log 2>&1


---

## ✏️ What I Learned

* How to use AWS CLI and S3 for backups
* How to write a simple shell script
* How to automate tasks using cron
* How to work with EC2 and Linux

---

## 💪 How This Helps Me

This is a practical cloud project that shows I can:

✅ Automate real system admin tasks
✅ Work with AWS services
✅ Use the command line to solve problems

---








