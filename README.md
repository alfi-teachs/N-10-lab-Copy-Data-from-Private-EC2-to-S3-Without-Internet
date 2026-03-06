# N-10-lab-Copy-Data-from-Private-EC2-to-S3-Without-Internet
Private EC2 (in private subnet) → S3 VPC Gateway Endpoint → S3  The traffic never goes to the internet. It stays inside the AWS network


# Traffic Flow:

# EC2 (Private Subnet) → VPC Endpoint → S3

# No Internet Gateway or NAT Gateway is required for S3 access.
