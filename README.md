### Installation Jenkins with Docker

#### Create bridge network

```bash
docker network create jenkins
```

#### Download and run docker:dind

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

#### Create & run container jenkins

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

---

### Or, use makefile to simplify installation

```bash
make add-network
make add-docker-dind
make build-jenkins
make run-jenkins
```

### Post-installation setup

1. Browse to http://127.0.0.1:8080/
   <img src="./docs/unloc-jenkins.png"/>
2. In terminal:

   ```bash
   # get password
   docker exec jenkins-blueocean cat /var/jenkins_home/secrets/initialAdminPassword
   ```

   Copy paste password to continue the setup wizard.
