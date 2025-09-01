## [AWS CLI Command Reference](https://awscli.amazonaws.com/v2/documentation/api/latest/index.html)

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
