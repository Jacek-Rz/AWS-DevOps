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
    --tag-specifications '{"ResourceType":"instance","Tags":[{"Key":"Name","Value":"INSTANCE NAME"}]}' \

- to list instances:
aws ec2 describe-instances
aws ec2 describe-instance-status

- to terminate and delete instance:
aws ec2 terminate-instances --instance-ids RUNNING-INSTANCE-ID
