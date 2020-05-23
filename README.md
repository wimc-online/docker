# agh-inz-roma

## development
```shell script
# check if docker compose is installed
command -v docker-compose
# set local environment variables
printf \
'LE_EMAIL=
TEST_DOMAIN=
API_DOMAIN=api.wimc.localhost
KEYCLOAK_DOMAIN=keycloak.wimc.localhost
' >> .env
# start local containers
docker-compose -f docker-compose.yml -f docker-compose.dev.yml up -d
# open traefik dashboard on http://localhost:8080, to verify services and routers
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
