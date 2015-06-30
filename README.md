# convox/cli [![Circle CI](https://circleci.com/gh/convox/cli/tree/master.svg?style=svg&circle-token=d24962574d368c687053bb89a4e2c185bc46ea0a)](https://circleci.com/gh/convox/cli/tree/master)

Convox command line interface.

## Installation

**OS X**

    $ curl -LO https://github.com/convox/cli/releases/download/v0.2/darwin-amd64-convox.tar.bz2 && tar xjvf darwin-amd64-convox.tar.bz2 && cp bin/darwin/amd64/convox /usr/local/bin

**Linux**

    $ curl -LO https://github.com/convox/cli/releases/download/v0.2/linux-amd64-convox.tar.bz2 && tar xjvf linux-amd64-convox.tar.bz2 && cp bin/linux/amd64/convox /usr/local/bin

**Golang**

    $ go get github.com/convox/cli/convox

## Usage

    $ convox help

## convox start

Start runs any app with [Docker Compose](https://docs.docker.com/compose/).

If `docker-compose.yml` and/or `Dockerfile` do not exist, start will create them
for you, then build and pull images.

    $ cd myapp
    $ convox start
    Procfile app detected. Writing Dockerfile, docker-compose.yml.
    Step 0 : FROM convox/cedar
    ...

    Trigger 7, RUN /buildkit/bin/detect /app /cache
    Step 0 : RUN /buildkit/bin/detect /app /cache
     ---> Running in d7bef824d297
    Buildkit+Ruby
    Trigger 8, RUN /buildkit/bin/compile /app /cache
    Step 0 : RUN /buildkit/bin/compile /app /cache
     ---> Running in 731df369192e
    -----> Compiling for Ruby
    ...

    Successfully built d4c4605f1f09
    [2015-06-23 17:31:42] INFO  WEBrick 1.3.1
    [2015-06-23 17:31:42] INFO  ruby 2.1.3 (2014-09-19) [x86_64-linux]
    == Sinatra (v1.4.6) has taken the stage on 3000 for production with backup from WEBrick
    [2015-06-23 17:31:42] INFO  WEBrick::HTTPServer#start: pid=1 port=3000

    $ curl $(boot2docker ip):5000
    Hello, World

Start will also help set up and debug your Docker / Boot2Docker environment when
it encounters problems.

## convox login

Login to your Convox API.

    $ convox login convox-424363854.us-east-1.elb.amazonaws.com
    Password: 
    Login Succeeded

## convox deploy

Deploy any app to AWS.

If `docker-compose.yml` and/or `Dockerfile` do not exist, deploy will create 
them for you, then build and pull images. Then deploy tags images and pushes 
them to your private registry and creates an app and release.

    $ cd myapp
    $ convox deploy
    Docker Compose app detected.
    web uses an image, skipping
    latest: Pulling from httpd
    ...

    Tagging httpd
    Pushing convox-424363854.us-east-1.elb.amazonaws.com:5000/myapp-web:1435598703
    ...

    Created app myapp6
    Status running
    Created release 1435598703
    Status running

## convox env

Inspect and edit environment variables.

    $ convox env set FOO=bar BAZ=qux

    $ convox env
    BAZ=qux
    FOO=bar

    $ convox env set FOO=quux CORGE=grault

    $ convox env
    BAZ=qux
    CORGE=grault
    FOO=quux

    $ convox env unset FOO

    $ convox env
    BAZ=qux
    CORGE=grault

    $ convox env get BAZ
    qux

## License

Apache 2.0 &copy; 2015 Convox, Inc.
