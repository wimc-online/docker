# agh-inz-roma

## development
```shell script
# prepare local environment variables
cp .env.dist .env
# check if docker compose is installed
command -v docker-compose
# start local containers
docker-compose -f docker-compose.yml -f docker-compose.dev.yml up --detach
# open router dashboard on https://user:password@router.wimc.localhost, to verify configuration
```

## deployment
```shell script
# connect to roma instance
ssh roma
# change working directory to roma repository
cd roma
# pull last changes for containers
git pull
# update images
docker-compose -f docker-compose.yml -f docker-compose.pro.yml pull
# update containers
docker-compose -f docker-compose.yml -f docker-compose.pro.yml up --detach
```

## EC2 instance access
```shell script
# generate new key pair in AWS EC2 panel, save .pem file (probably ~/.ssh/ is the best option) and update below variable accordingly
pemPath=~/.ssh/roma-XXX.pem
# find EC2 roma instance Public DNS (IPv4) and update with it variable below
romaDns=YYY.eu-central-1.compute.amazonaws.com
# ensure private key don't have too wide permissions
chmod 600 $pemPath
# generate public key from private key
ssh-keygen -f $pemPath -y >> ~/.ssh/${pemPath%.pem}.pub
# ask administrator to add generated public key to ~/.ssh/authorized_keys on roma instance
# add roma ssh configuration for easier connection
printf '
Host roma
  HostName %s
  User ec2-user
  IdentityFile %s
' $romaDns $pemPath >> ~/.ssh/config
# test ssh connection
ssh roma
```

## misc
- generate basicauth entry
```shell script
# check if htpasswd is installed
command -v htpasswd
# generate htpasswd entry
echo $(htpasswd -nb user password)
```
