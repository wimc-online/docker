# Docker for "Where is my courier?"
![Deploy to host](https://github.com/wimc-online/docker/workflows/Deploy%20to%20host/badge.svg)

## Development
```shell script
# check if docker is installed
command -v docker
# check if docker compose is installed
command -v docker-compose
# login to github packages with personal access token
docker login https://docker.pkg.github.com
# prepare local env variables
cp .env.dev.dist .env
# prepare aliases
source .aliases
# update images
wimcdc pull
# start local containers
wimcdc up -d
```

## Darwin
```shell script
# install docker-sync
sudo gem install docker-sync -n /usr/local/bin
# start docker-sync
docker-sync start
# add domains to /etc/hosts
echo '127.0.0.1 api.wimc.localhost app.wimc.localhost auth.wimc.localhost router.wimc.localhost' >> /etc/hosts
```

## Troubleshooting
```shell script
# api keep restarting
wimcdc pull api && wimcdc up -d --force-recreate api
# clear docker-sync cache
docker-sync stop && docker-sync clean && docker-sync start
```

## Links
- [About GitHub Packages - about tokens](https://help.github.com/en/packages/publishing-and-managing-packages/about-github-packages#about-tokens)
- [Compose file version 3 reference](https://docs.docker.com/compose/compose-file/)
