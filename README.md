### Installation Jenkins with docker

#### Create bridge network

```bash
docker network create jenkins
```

#### Download and run the docker:dind

```bash
docker run \
  --name jenkins-docker \
  --rm \
  --detach \
  --privileged \
  --network jenkins \
  --network-alias docker \
  --env DOCKER_TLS_CERTDIR=/certs \
  --volume jenkins-docker-certs:/certs/client \
  --volume jenkins-data:/var/jenkins_home \
  --publish 2376:2376 \
  docker:dind \
  --storage-driver overlay2
```

#### Customize the official Jenkins Docker image from Dockerfile, and assign the image a meaningful name, such as "solehudin5699/jenkins-blueocean:2.440.1-1"

```bash
docker build -t solehudin5699/jenkins-blueocean:2.440.1-1 .
```

#### Run solehudin5699/jenkins-blueocean:2.440.1-1 as a container

```bash
docker run \
  --name jenkins-blueocean \
  --restart=on-failure \
  --detach \
  --network jenkins \
  --env DOCKER_HOST=tcp://docker:2376 \
  --env DOCKER_CERT_PATH=/certs/client \
  --env DOCKER_TLS_VERIFY=1 \
  --publish 8080:8080 \
  --publish 50000:50000 \
  --volume jenkins-data:/var/jenkins_home \
  --volume jenkins-docker-certs:/certs/client:ro \
  solehudin5699/jenkins-blueocean:2.440.1-1
```

### Post-installation setup

1. Browse to http://localhost:8080
   <img src="./docs/unloc-jenkins.png"/>
2. In terminal:

   ```bash
   docker exec -it jenkins-blueocean /bin/sh

   # get password in container:
   cat /var/jenkins_home/secrets/initialAdminPassword
   ```

   Copy paste password to continue the setup wizard.