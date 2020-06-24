# Docker for "Where is my courier?"

## Prerequisites
```shell script
# check if docker is installed
command -v docker
# check if docker compose is installed
command -v docker-compose
# login to github packages with personal access token
docker login https://docker.pkg.github.com
```

## Development
```shell script
# pull last changes for repository
git pull
# update images
docker-compose -f docker-compose.yml -f docker-compose.dev.yml pull
# start local containers
docker-compose -f docker-compose.yml -f docker-compose.dev.yml up --detach
# open router dashboard on https://user:password@router.wimc.localhost, to verify configuration
```

## Misc
- generate basicauth entry
```shell script
# check if htpasswd is installed
command -v htpasswd
# generate htpasswd entry
echo $(htpasswd -nb user password)
```

## Links
- [About GitHub Packages - about tokens](https://help.github.com/en/packages/publishing-and-managing-packages/about-github-packages#about-tokens)
- [Compose file version 3 reference](https://docs.docker.com/compose/compose-file/)
