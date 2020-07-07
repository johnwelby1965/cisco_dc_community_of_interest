README.md
---------

## Overview
In the `.gitlab-ci.yml` file we reference a Docker Image to execute the commands. These steps were used (along with the `Dockerfile` in this directory).

### Build the image
The `Dockerfile` installs Ansible and Ansible Lint. Because we are using `ansible-galaxy` to install the ACI collection, we need  Ansible 2.9.x to properly install the collection and Lint the playbook(s).

```bash
docker build -f ./Dockerfile -t joelwking/ansible:1.0 .
```
Run the docker image locally to verify it has built and executes correctly.

```bash
docker run -it joelwking/ansible:1.0
```
The output from running the image should show the version of Ansible installed.

```bash
administrator@olive-iron:~/docker/cicd$ docker run -it joelwking/ansible:1.0
ansible-playbook 2.9.10
  config file = /etc/ansible/ansible.cfg
  configured module search path = [u'/root/.ansible/plugins/modules', u'/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/local/lib/python2.7/dist-packages/ansible
  executable location = /usr/local/bin/ansible-playbook
  python version = 2.7.17 (default, Apr 15 2020, 17:20:14) [GCC 7.5.0]
```
View the charastics of the image

```bash
administrator@olive-iron:~/docker/cicd$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
joelwking/ansible   1.0                 f1ce7db4e4eb        3 minutes ago       813MB
```
### Publish the image
In order to reference this Docker image in the `.gitlab-ci.yml` pipeline file, we need to publish the image on Docker Hub. First login from the machine which build the image.
```bash
docker login --username=joelwking
```
#### Create the repository on DockerHub
Log on your DockerHub account and create the repo name. For example, https://hub.docker.com/repository/docker/joelwking/ansible/general.

>**Note**: in this example, we specified the repository name `joelwking/ansible` and tag `1.0` when we built the image. However, it can be done separately as shown.
```bash
docker tag f1ce7db4e4eb joelwking/ansible:1.0
```
Now push the image to Docker Hub.
```bash
docker push joelwking/ansible
```
We can now reference this image in the `.gitlab-ci.yml` file.

## References
https://ropenscilabs.github.io/r-docker-tutorial/04-Dockerhub.html

## Author
Joel W. King  @joelwking