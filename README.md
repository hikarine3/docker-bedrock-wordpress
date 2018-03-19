# What is this document
This is for creating initail environment of developing Wordpress env with bedrock + docker.
You can use this for initiation of your WP project.

# Used software
- docker-compose
- Mysql 5.7
- PHP 7.2
- Wordpress latest
- Bedrock (Fully coposer based WP dev env)

# How to run docker env

## Kill all running process
docker kill $(docker ps -q);

## Delete all stopped containers (including data-only containers)
docker rm $(docker ps -a -q);

## Use docker
docker compose up;

## If you want customize
### Edit docker-compose.yml before "docker up"


## If you want to change salt
cd bedrock;

wp dotenv salts regenerate

## Prepare necessary php modules
cd bedrock;

composer.phar update;

## Add wordpress module
Edit bedrock/composer.json referecing to https://wpackagist.org

## Confirm web sit
Access to http://localhost/

## Edit source code
Edit files under bedrock/web/app

# Reference
## How this repository is created
composer.phar create-project roots/bedrock bedrock;

cd bedrock;

wp package install aaemnnosttv/wp-cli-dotenv-command

wp dotenv salts regenerate


# License

MIT

# Author

Hajime Kurita

An adminstrator of https://sakuhindb.com/ , http://minakoe.jp/ and so on

https://twitter.com/hikarine3

https://en.sakuhindb.com/pe/Administrator/

https://github.com/hikarine3

https://cloud.docker.com/swarm/1stclass/repository/list

https://www.npmjs.com/package/@hikarine3/

http://search.cpan.org/~hikarine/
