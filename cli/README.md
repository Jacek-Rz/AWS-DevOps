## [AWS CLI Command Reference](https://awscli.amazonaws.com/v2/documentation/api/latest/index.html)

### Creating instance
[Needed CLI Commands](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/ec2/index.html)
You work with EC2.
Things to do:
1. Generate key pair to log into the instance
aws ec2 create-key-pair --key-name "KEY NAME" --query "KeyMaterial" --output text > FILE.pem
- to list all key pairs:
aws ec2 describe-key-pairs
or to list just one:
aws ec2 describe-key-pairs --key-name "KEY NAME"
- to delete key pair:
aws ec2 delete-key-pair --key-name "KEY NAME"
or
aws ec2 delete-key-pair --key-pair-id KEY-PAIR-ID

2. Get default VPC ID:
aws ec2 describe-vpcs --filters Name=instance-tenancy,Values=default --query Vpcs[*].[VpcId] --output text

3. Create security groups to allow connections to some ports:
- to add security group:
aws ec2 create-security-group --description "DESCRIPTION" --group-name "GROUP NAME" --vpc-id DEFAULT-VPC-ID

- to add allowed port to security group:
aws ec2 authorize-security-group-ingress --group-id "CREATED-SECURITY-GROUP-ID" \
    --ip-permissions \
    '{"IpProtocol":"tcp","FromPort":FIRST-PORT,"ToPort":LAST-PORT,"IpRanges":[{"CidrIp":"0.0.0.0/0"}]}'

- to list all security groups:
aws ec2 describe-security-groups

- to revoke allowed port from security group:
aws ec2 revoke-security-group-ingress --group-name "launch-wizard-1" --ip-permissions '{"IpProtocol":"tcp","FromPort":80,"ToPort":80,"IpRanges":[{"CidrIp":"0.0.0.0/0"}]}'

- to delete secruity group:
aws ec2 delete-security-group --group-name "GROUP NAME"
or
aws ec2 delete-security-group --group-id GROUP-ID

4. Launch instance:
- find e.g. Windows image ID:
aws ec2 describe-images --filters Name=platform-details,Values=Windows --query Images[*].[Name,SourceImageId] --output text

- to run instance:
aws ec2 run-instances --image-id "ami-0fcb037987698aecb" --instance-type "t2.micro" --key-name "KEY-NAME" \
    --network-interfaces '{"AssociatePublicIpAddress":true,"DeviceIndex":0,"Groups":["sg-preview-1"]}' \
    --tag-specifications '{"ResourceType":"instance","Tags":[{"Key":"Name","Value":"INSTANCE NAME"}]}'

- to list instances:
aws ec2 describe-instances
aws ec2 describe-instance-status

- to terminate and delete instance:
aws ec2 terminate-instances --instance-ids RUNNING-INSTANCE-ID

### Creating own VPC and subnets
1. Create own VPC:
- creating 10.20.0.0/16 VPC:
aws ec2 create-vpc --cidr-block 10.20.0.0/16 --tag-specifications '{"ResourceType":"vpc","Tags":[{"Key":"Name","Value":"MY VPC 01"}]}'

- to list VPC:
aws ec2 describe-vpcs

- to delete VPC:
aws ec2 delete-vpc --vpc-id MY-VPC-ID

2. Create subnets:
- creating 10.20.0.0/24 subnet on availability zone eu-west-1a:
aws ec2 create-subnet --vpc-id MY-VPC-ID --cidr-block 10.20.0.0/24 --availability-zone eu-west-1a --tag-specifications '{"ResourceType":"subnet","Tags":[{"Key":"Name","Value":"My-Subnet-01"}]}'
- creating 10.20.1.0/24 subnet on availability zone eu-west-1b:
aws ec2 create-subnet --vpc-id MY-VPC-ID --cidr-block 10.20.1.0/24 --availability-zone eu-west-1b --tag-specifications '{"ResourceType":"subnet","Tags":[{"Key":"Name","Value":"My-Subnet-02"}]}'

- to list subnets:
aws ec2 describe-subnets

- to delete subnet:
asws ec2 delete-subnet --subnet-id MY-SUBNET-ID

3. Create internet-gateway:
- creating gateway:
aws ec2 create-internet-gateway --tag-specifications '{"ResourceType":"internet-gateway","Tags":[{"Key":"Name","Value":"MY-Internet-Gateway"}]}'

- to list internet-gateways:
aws ec2 describe-internet-gateways

- to delete internet-gateway:
aws ec2 delete-internet-gateway --internet-gateway-id MY-IG-ID

4. Attach internet-gateway to VPC:
- to attach:
aws ec2 attach-internet-gateway --internet-gateway-id MY-GATEWAY-ID --vpc-id MY-VPC-ID

- to detach:
aws ec2 detach-internet-gateway  --internet-gateway-id MY-GATEWAY-ID --vpc-id MY-VPC-ID

4. Create routing tables:
- to create:
aws ec2 create-route-table --vpc-id MY-VPC-ID --tag-specifications '{"ResourceType":"route-table","Tags":[{"Key":"Name","Value":"MY-ROUTE-TABLE"}]}'

- to list routing tables:
aws ec2 describe-route-tables

- to delete routing tables:
aws ec2 delete-route-table --route-table-id

- to change name:
aws ec2 create-tags --resources MY-ROUTE-TABLE-ID --tags Key=Name,Value="MY-NEW-NAME"

- to add route:
aws ec2 create-route --route-table-id MY-ROUTE-TABLE --destination-cidr-block 0.0.0.0/0 --gateway-id MY-INTERNET-GATEWAY

- to associate subnet to route-table:
aws ec2 associate-route-table --route-table-id MY-ROUTE-TABLE-ID --subnet-id MY-SUBNET-ID

4. S3 copy local to remote
- copy one file:
aws s3 cp /path/to/local/file s3://my-bucket-name/destination/path
- many files:
aws s3 cp /path/to/local/files s3://my-bucket-name/destination/path --recursive --exclude "*" --include "file1.jpg" --include "*.pdf"
- directory:
aws s3 cp /path/to/local/directory s3://my-bucket-name/destination/path --recursive

5. S3 copy remote to local
- all files with JPG extension from all directories to local current directory:
aws s3 cp s3://my-bucket-name . --recursive --exclude "*" --include "*.JPG"
