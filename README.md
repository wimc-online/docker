# agh-inz-roma

## configuration
Install [aws cli](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html) and [ecs cli](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ECS_CLI_installation.html).
Ask for accessKeys.csv and export AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY.
```shell script
# aws cli
aws configure --profile roma
# ecs cli
ecs-cli configure --cluster roma --default-launch-type EC2 --config-name roma --region eu-central-1
ecs-cli configure profile --access-key $AWS_ACCESS_KEY_ID --secret-key $AWS_SECRET_ACCESS_KEY --profile-name roma
```

## services management
You can omit `--project-name roma` if you have this repository inside `roma` directory.
You can omit `--cluster-config roma --ecs-profile roma` if `roma` profile is your default ecs config and profile.
```shell script
# start services
ecs-cli compose service up --create-log-groups --project-name roma --cluster-config roma --ecs-profile roma
# list services
ecs-cli compose service ps --project-name roma --cluster-config roma --ecs-profile roma
# stop services
ecs-cli compose service down --project-name roma --cluster-config roma --ecs-profile roma
```

## cluster management
You can omit `--cluster-config roma --ecs-profile roma` if `roma` profile is your default ecs config and profile.
```shell script
# remove old cluster if needed
ecs-cli down --force --cluster-config roma --ecs-profile roma
# create new cluster and retrieve subnet ids and VPC_ID
ecs-cli up --capability-iam --instance-type t2.micro --enable-service-discovery --cluster-config roma --ecs-profile roma
# change subnet ids and security_group id in ecs-params.yml
```

## servicediscovery management
```shell script
# create a public namespace
aws servicediscovery create-public-dns-namespace --name wimc.online
# retrieve namespace id from operation result
aws servicediscovery get-operation --operation-id $OPERATION_ID
# change public_dns_namespace id in ecs-params.yml
```
