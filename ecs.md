# ECS
For now we don't use Elastic Container Service on AWS for deployment, but maybe it will be useful in the future.

## configuration
Install [aws cli](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html), [ecs cli](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ECS_CLI_installation.html) nad [jq](https://stedolan.github.io/jq/download/).
Ask for accessKeys.csv and export AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY.
```shell script
# aws cli
aws configure --profile roma
# ecs cli
ecs-cli configure profile --profile-name roma \
                          --access-key $AWS_ACCESS_KEY_ID \
                          --secret-key $AWS_SECRET_ACCESS_KEY
ecs-cli configure --cluster roma \
                  --default-launch-type EC2 \
                  --config-name roma \
                  --region eu-central-1
```

## cluster, target group, load balancer, hosted zone and service creation
```shell script
# generate private key
aws ec2 create-key-pair --key-name roma | jq -r '.KeyMaterial' > ~/.ssh/id_rsa_roma

# create cluster
ecs-cli up --cluster roma \
           --keypair roma \
           --capability-iam \
           --instance-type t2.micro
ROMA_VPC=
ROMA_SG=
ROMA_SUBNET_1=
ROMA_SUBNET_2=

# create load balancer
ROMA_LB_ARN=$(aws elbv2 create-load-balancer --name roma  \
                                             --security-groups $ROMA_SG \
                                             --subnets $ROMA_SUBNET_1 $ROMA_SUBNET_2 \
                                             | jq -r '.LoadBalancers[0].LoadBalancerArn')

# create target group
ROMA_TG_API_ARN=$(aws elbv2 create-target-group --name api \
                                                --protocol HTTP \
                                                --port 80 \
                                                --vpc-id $ROMA_VPC \
                                                | jq -r '.TargetGroups[0].TargetGroupArn')

# create target group listener
ROMA_TL_API_ARN=$(aws elbv2 create-listener --load-balancer-arn $ROMA_LB_ARN \
                                            --protocol HTTP \
                                            --port 80  \
                                            --default-actions Type=forward,TargetGroupArn=$ROMA_TG_API_ARN \
                                            | jq -r '.Listeners[0].ListenerArn')

# create hosted zone
ROMA_DOMAIN=
CALLER_REF=$(date +"%s")
aws route53 create-hosted-zone --name $ROMA_DOMAIN --caller-reference $CALLER_REF

# Set DNS server for domain and configure Amazon Route 53 to route traffic to an ELB load balancer
# https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-to-elb-load-balancer.html

# start service
ecs-cli compose --cluster roma \
                --project-name roma \
                service up \
                --target-group-arn $ROMA_TG_API_ARN \
                --container-name api \
                --container-port 80

# stop service
ecs-cli compose --cluster roma \
                --project-name roma \
                service down
```
