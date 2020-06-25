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
cp .env.dist .env
# update images
docker-compose -f docker-compose.yml -f docker-compose.dev.yml pull
# start local containers
docker-compose -f docker-compose.yml -f docker-compose.dev.yml up --detach
```

## Links
- [About GitHub Packages - about tokens](https://help.github.com/en/packages/publishing-and-managing-packages/about-github-packages#about-tokens)
- [Compose file version 3 reference](https://docs.docker.com/compose/compose-file/)
