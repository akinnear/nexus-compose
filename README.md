Used as a guide: https://blog.sonatype.com/using-nexus-3-as-your-repository-part-3-docker-images

Create volume:
docker volume create --name nexus-data

run:
docker-compose -f "docker-compose.yml" up -d --build

Get password by running:
docker run --rm -it -v nexus-data:/volume -w /volume ubuntu:20.04

the password file is in the base of the directory

Create nexus blobs:
docker-external
docker-private
docker-group

Create hosted nexus docker repositories check http port:
docker-external   8083
docker-private    8084

Created group nexus docker repository check http port:
docker-group      8082

Set docker demon json file to, use own name of server/machine:
{
  "registry-mirrors": [
    "http://192.168.0.149:8082"
  ],
  "insecure-registries": [
    "192.168.0.149:8082",
    "192.168.0.149:8083",
    "192.168.0.149:8084"
  ],
  "debug": true,
  "experimental": false
}

restart docker

login on client:
docker login 192.168.0.149:8082
docker login 192.168.0.149:8083
docker login 192.168.0.149:8084

Tag images with external repo:
docker tag ubuntu:20.04 192.168.0.149:8083/ubuntu:20.04

Push to repo:
docker push 192.168.0.149:8083/ubuntu:20.04

Pull from repo:
docker pull 192.168.0.149:8082/ubuntu:20.04

Note right now the registry-mirrors is not working

close: 
docker-compose -f "docker-compose.yml" down