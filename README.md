# cfn-stackset-example

account A: 
- administration role: cfn-admin-role
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "",
            "Effect": "Allow",
            "Principal": {
                "Service": "cloudformation.amazonaws.com"
            },
            "Action": "sts:AssumeRole"
        }
    ]
}
```

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "sts:AssumeRole"
            ],
            "Resource": "arn:aws:iam::251285041063:role/cfn-instance-role"
        }
    ]
}

``


account B:
- execution role: cfn-instance-role
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "",
            "Effect": "Allow",
            "Principal": {
                "Service": "cloudformation.amazonaws.com"
            },
            "Action": "sts:AssumeRole"
        },
        {
            "Sid": "",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::525261692294:role/cfn-admin-role"
            },
            "Action": "sts:AssumeRole"
        }
    ]
}
```

```
arn:aws:iam::aws:policy/AmazonS3FullAccess

arn:aws:iam::aws:policy/AmazonSNSFullAccess

arn:aws:iam::aws:policy/AWSCloudFormationFullAccess
```


## Create the StackeSet and Stack Instance with creation an S3 bucket

```
aws --profile 2022 --region us-east-1 \
 cloudformation create-stack-set \
    --stack-set-name cfn-create-s3 \
    --template-url https://s3.amazonaws.com/cf-template-2022-us-east-1/s3.yml \
    --description "testing s3 creation with stackset" \
    --administration-role-arn 'arn:aws:iam::525261692294:role/cfn-admin-role' \
    --execution-role-name 'cfn-instance-role'

aws --profile 2022 --region us-east-1 \
cloudformation delete-stack-set --stack-set-name cfn-create-s3

aws --profile 2022 --region us-east-1 \
cloudformation list-stack-sets

aws --profile 2022 --region us-east-1 \
 cloudformation create-stack-instances \
  --stack-set-name cfn-create-s3 \
  --accounts '["251285041063"]' \
  --regions '["us-east-1"]' \
  --operation-preferences FailureToleranceCount=0,MaxConcurrentCount=1


aws --profile 2022 --region us-east-1 \
cloudformation describe-stack-set-operation \
  --stack-set-name cfn-create-s3 \
  --operation-id "7739d4ba-57bd-4dbb-a03c-24a41d004e96"

```
