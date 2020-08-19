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
# update images
docker-compose -f docker-compose.yml -f docker-compose.local.yml -f docker-compose.dev.yml -f docker-compose.dev.$(uname -s).yml pull
# start local containers
docker-compose -f docker-compose.yml -f docker-compose.local.yml -f docker-compose.dev.yml -f docker-compose.dev.$(uname -s).yml up --detach
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

## Links
- [About GitHub Packages - about tokens](https://help.github.com/en/packages/publishing-and-managing-packages/about-github-packages#about-tokens)
- [Compose file version 3 reference](https://docs.docker.com/compose/compose-file/)
