# agh-inz-roma

## services management
Install [ecs cli](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ECS_CLI_installation.html).
You can omit ` --cluster-config roma --ecs-profile roma` if `roma` profile is your default ecs config and profile.
```shell script
# configure ecs cli
ecs-cli configure --cluster roma --default-launch-type FARGATE --config-name roma --region eu-central-1
ecs-cli configure profile --access-key $AWS_ACCESS_KEY_ID --secret-key $AWS_SECRET_ACCESS_KEY --profile-name roma
# start services
ecs-cli compose service up --create-log-groups --cluster-config roma --ecs-profile roma
# list services
ecs-cli compose service ps --cluster-config roma --ecs-profile roma
# stop services
ecs-cli compose service down --cluster-config roma --ecs-profile roma
```

## cluster management
Ask for accessKeys.csv and export AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY.
Install [aws cli](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html).
```shell script
# configure aws cli with AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY, eu-central-1, None
aws configure --profile roma
# remove old cluster if needed
ecs-cli down --force --cluster-config roma --ecs-profile roma
# create new cluster and retrieve subnet ids and VPC_ID
ecs-cli up --cluster-config roma --ecs-profile roma
# retrieve security_group_id with VPC_ID from GroupId field
aws ec2 describe-security-groups --filters Name=vpc-id,Values=$VPC_ID --profile roma
# change subnet ids and security_group id in ecs-params.yml
# add a security group rule to allow inbound access
aws ec2 authorize-security-group-ingress --group-id $security_group_id --protocol tcp --port 80 --cidr 0.0.0.0/0
```
