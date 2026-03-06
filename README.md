# N-10-lab-Copy-Data-from-Private-EC2-to-S3-Without-Internet
Private EC2 (in private subnet) → S3 VPC Gateway Endpoint → S3  The traffic never goes to the internet. It stays inside the AWS network


# Traffic Flow:

# EC2 (Private Subnet) → VPC Endpoint → S3

No Internet Gateway or NAT Gateway is required for S3 access.

# Step 1: Create VPC

Create a VPC.

Example configuration:

CIDR: 10.0.0.0/16

Create two subnets.

Public Subnet
10.0.1.0/24

Private Subnet
10.0.2.0/24


# Step 2: Create Route Tables

Public Route Table

Destination     Target
0.0.0.0/0       Internet Gateway


Private Route Table

Local routes only


Associate the private subnet with the private route table.

# Step 3: Launch EC2 Instance

Launch EC2 in the private subnet.

Configuration example:

AMI: Amazon Linux

Subnet: Private Subnet

Public IP: Disabled

Security Group: Allow SSH (from bastion if needed)

Attach an IAM Role with S3 access.

Policy example:

AmazonS3FullAccess
