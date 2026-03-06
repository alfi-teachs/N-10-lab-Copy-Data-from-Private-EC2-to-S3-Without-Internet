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

# Step 4: Create S3 Bucket

Create a bucket in Amazon S3.

Example name:

my-private-data-bucket

# Step 5: Create VPC Endpoint

Go to VPC → Endpoints → Create Endpoint.

Configuration:

Service:

com.amazonaws.region.s3


Type:

Gateway Endpoint


Select:

Your VPC

Private Route Table

AWS automatically adds a route to the route table.

Example route added:

pl-xxxxxxx → vpce-xxxxxxx

# Step 6: Connect to Private EC2

SSH into the instance.

Example:

ssh -i key.pem ec2-user@private-ip

# Step 7: Create a Test File
echo "Hello from Private EC2" > test.txt

# Step 8: Copy File to S3

Upload the file.

aws s3 cp test.txt s3://my-private-data-bucket/

# Step 9: Verify Upload

Check the bucket.

aws s3 ls s3://my-private-data-bucket/


You should see:

test.txt

# Result

The private EC2 instance successfully uploaded data to S3 using a VPC Endpoint without accessing the public internet.

# Key Learning

Secure service communication inside AWS

Using Gateway Endpoint for S3

Private subnet architecture

Data transfer without NAT or Internet Gateway
