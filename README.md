
# Usage
## Running Container as a Command

To run this container using the [rimusz/docker-siege](https://hub.docker.com/r/rimusz/docker-siege/) image stored in Docker Hub that is automatically built whenever this github repository is updated. The [Dockerfile](Dockerfile) has a default `ENTRYPOINT` of `siege` and all arguments passed after the container image will pass to `siege`. The default argument is `--help`.

## Running Container With Local Configuration

To skip the bundled configuration files in the image, and use local files in the current directoy, bind mount it to `/app` using the `-v` flag.

For example using this repository and you want to create a URL list to pass to siege called `urls.txt` without having to re-build the container image,

```
docker pull rimusz/docker-siege
docker run -it --rm -v $(pwd):/app rimusz/docker-siege -c 50 -f /app/urls.txt -R /app/siege.conf
```

The `-v $(pwd):/app` will bind mount your current directory to `/app` within the container, letting you use local files.

## Running in Kubernetes

You can run it as Kubernetes pod which will delete itself when finished:

```
kubectl run --generator=run-pod/v1 siege --rm -it --image rimusz/docker-siege -- -c 100 -r 100 -b https://mysite.com
```

## Updating Docker Image

To update the version fo Siege, find the latest version from the [Siege Downloads](http://download.joedog.org/siege/) page and in the [Dockerfile](Dockerfile) update the `SIEGE_VERSION` variable and re-build,

```
docker build -t rimusz/docker-siege .
```
