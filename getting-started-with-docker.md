Title: Getting started with Docker for your aah application
Desc: Getting started with Docker for your aah application.
Keywords: docker, container, aah, application
---
# Getting started with Docker

Great news!! <span class="badge lb-sm">Since v0.10</span> release aah provides Docker Image on [Docker Hub](https://hub.docker.com/r/aahframework/) and aah CLI command to generate `Dockerfile` config.

aah has automated build process for Docker Image creation. Supported Docker tags are -

  * `aahframework/aah:latest` - Latest production release.
  * `aahframework/aah:{version}` - Specific release version, for e.g.: `aahframework/aah:v0.9`.
  * `aahframework/aah:edge` - Latest development edge version.

<div class="alert alert-info-blue">
<p><strong>Note:</strong>
<ul>
  <li>aah docker base image is <code>golang:latest</code>.</li>
  <li>Ensure to install <a href="https://www.docker.com/get-docker">Docker</a> on your machine.</li>
</ul>
</p>
</div>

**Table of Contents**

  * [Pull aah Docker Image](#pull-aah-docker-image)
  * [Handy `generate` Command](#handy-generate-command)
  *

## Pull aah Docker Image

Pull docker image from Docker Hub.

```bash
# Pull latest production release
docker pull aahframework/aah:latest

# Pull edge version
docker pull aahframework/aah:edge

# Pull specific version
docker pull aahframework/aah:v0.9
```

## Handy `generate` Command

aah CLI includes `generate` command to help developers in their application development path. This command generates `two` docker files -

  * `Dockerfile.dev` - For development usage with aah docker image.
  * `Dockerfile.prod` - Using Multistage-build to create very small docker image for production purpose. More info, refer to https://docs.docker.com/develop/develop-images/multistage-build/.

<div class="alert alert-info-blue">
<p><strong>Note:</strong> aah CLI provides very good starter Dockerfile(s) for your application, enhance it per your need.</p>
</div>

#### Example
```bash
# run from anywhere
aah generate script --name docker --importpath github.com/user/appname

# OR if you're in app directory or path
aah generate script --name docker

# OR short form
aah g s -n docker
```

## Using Dockerfile.*

#### To Build Docker Image

```bash
# Using Dockerfile.dev
docker build --no-cache -t "yourapp:dev" -f Dockerfile.dev .

# Using Dockerfile.prod
docker build --no-cache -t "yourapp:latest" -f Dockerfile.prod .
```

#### Running built Docker Image

```bash
# To run Dockerfile.dev image
# To stop press Ctrl + C
docker run -it -p 8080:8080 "yourapp:dev" aah run

# To run Dockerfile.prod image
# To stop, do it via container stop
docker run -p 8080:8080 "yourapp:latest"
```

## Compile your app inside the Docker container

There might a occasions you would like to build your application inside the container and get the build artifact. You can write something like:

```bash
docker run --rm -v "$PWD":/go/src/github.com/user/appname -w /go/src/github.com/user/appname aahframework/aah:latest aah build
```
