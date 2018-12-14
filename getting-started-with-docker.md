Title: Getting started with Docker for your aah application
Desc: Getting started with Docker for your aah application.
Keywords: docker, container, aah, application
---
# Getting started with Docker

aah CLI provides command to generate `Dockerfile` for development and production purpose. aah provides very good starter Dockerfile(s) for your application, enhance it per your use case.

Ensure to install <a href="https://www.docker.com/get-docker">Docker</a> on your machine.

<div class="alert alert-info-blue">
<p><strong>Note:</strong> Since <code>v0.12.0</code> aah does not provide docker images <code>aahframework/aah:*</code> in-favor of Go modules.</p>
</div>

**Table of Contents**

  * [Handy `generate` Command](#handy-generate-command)
  * [Using Dockerfile.*](#using-dockerfile-dev-prod)

## Handy `generate` Command

Introduced in <span class="badge lb-sm">v0.10.0</span> aah CLI. Command `generate` is to help developer(s) in their application development path. It generates `two` docker files -

  * `Dockerfile.dev` - For development purpose.
  * `Dockerfile.prod` - Uses [Multistage-build](https://docs.docker.com/develop/develop-images/multistage-build/) to create very `tiny` docker image for production purpose.

### Example

```bash
# Go to application base directory and run
aah generate script --name docker

# OR shorthand
aah g s -n docker
```

## Using Dockerfile.{dev, prod}

Various sample usage commands.

#### To Build Docker Image

```bash
# Using Dockerfile.dev
docker build --no-cache -t "your-app-image-name:dev" -f Dockerfile.dev .

# Using Dockerfile.prod
docker build --no-cache -t "your-app-image-name:latest" -f Dockerfile.prod .
```

#### Running built Docker Image

```bash
# Run docker image created using `Dockerfile.dev`
# To stop: press Ctrl + C
docker run -it -p 8080:8080 "your-app-image-name:dev" aah run

# Run docker image created using `Dockerfile.prod`
# To stop: do it via container stop
docker run -p 8080:8080 "your-app-image-name:latest"
```
