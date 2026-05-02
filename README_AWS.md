# AWS Cloud Student Project Guide

This project is a skeleton designed for students taking AWS courses. It demonstrates a full-stack application lifecycle: **VPC -> EC2 (Frontend/Backend) -> S3/DynamoDB (Storage)**.

## 0. VPC (Virtual Private Cloud) - Networking First
Before deploying your compute, you must set up the network.
- **Action**: Create a VPC with at least one **Public Subnet**.
- **Internet Gateway**: Attach an IGW to your VPC and update the Route Table to allow outbound traffic (`0.0.0.0/0`).
- **Security Group**: Create a Security Group for your EC2 instance that allows **Inbound HTTP (Port 80/3000)** and **SSH (Port 22)**.

## 1. IAM (Identity and Access Management)
Your EC2 instance needs permissions to talk to S3 and DynamoDB.
- **Action**: Create an IAM Role for EC2.
- **Policies**: `AmazonS3FullAccess` and `AmazonDynamoDBFullAccess`.
- **Constraint**: The role should be attached **only to the EC2 instance**. Do not generate "User" Access Keys. This teaches the best practice of "Instance Profiles".

## 2. Amazon S3 (Simple Storage Service)
The app currently saves images to the `/uploads` folder.
- **AWS Integration**: Use the `@aws-sdk/client-s3`.
- **Flow**: User uploads image -> Express handles buffer -> Express uploads to S3 -> S3 returns Public URL.

## 3. Amazon DynamoDB or RDS
- **Metadata**: Store user name, age, and position. 
- **RDS (PostgreSQL/MySQL)**: Good for relational data. Place RDS in a **Private Subnet** for better security (advanced student task).

## 4. Amazon EC2 (Elastic Compute Cloud)
- **AMI**: Use a standard **Amazon Linux 2023** AMI.
- **Deployment**:
  1. SSH into instance.
  2. Install Node.js & Git.
  3. Clone repo and run `npm install`.
  4. Run `npm run build`.
  5. Start the server: `npm start`.

---

### Pro-Tips for Students:
- **Environment Variables**: Use `.env` files for bucket names and database names.
- **Monitoring**: Check **CloudWatch Logs** if your app crashes on EC2.
- **Cost**: Remember to delete your RDS/EC2 instances after the course to avoid charges!
