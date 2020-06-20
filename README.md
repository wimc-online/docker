# Docker for "Where is my courier?"

## Development
```shell script
# prepare local environment variables
cp .env.dist .env
# check if docker compose is installed
command -v docker-compose
# start local containers
docker-compose -f docker-compose.yml -f docker-compose.dev.yml up --detach
# open router dashboard on https://user:password@router.wimc.localhost, to verify configuration
```

## Deployment
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

## Misc
- generate basicauth entry
```shell script
# check if htpasswd is installed
command -v htpasswd
# generate htpasswd entry
echo $(htpasswd -nb user password)
```
