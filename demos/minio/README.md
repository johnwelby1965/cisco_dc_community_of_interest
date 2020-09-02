Docker and MinIO
------------------------------------
# MinIO

Refer to: https://min.io/product

> __MinIO is best suited for high performance tasks such as analytics, machine learning and modern, high throughput application development. ... As an object store, MinIO can store unstructured data such as photos, videos, log files, backups and container / VM images. Size of an object can range from a few KBs to a maximum of 5TB.__

Amazon S3 or Amazon Simple Storage Service is a service offered by Amazon Web Services that provides object storage through a web service interface.

In the companion playbook `minio.yml`, we illustrate using the Ansible `aws_s3` module to upload and download files to a MinIO instance running in a Docker container.

This capability provides a means for an organization to use both an internally hosted object store provided by MinIO as well as the AWS pubic cloud service Amazon S3 to store and retrieve configuration data for playbooks.

# Deploy MINIO using Docker
These steps will guide you to installing the MinIO docker image.

## Add your account to the docker group
```
sudo usermod -aG docker $USER
```

## Allow inbound connection on port 9000

```
sudo iptables -A INPUT -p tcp --dport 9000 -j ACCEPT 
service iptables restart
```
## docker image pull

Pull an image or a repository from a registry

```bash
$ docker pull minio/minio
```

```bash
administrator@olive-iron:~$ docker pull minio/minio:latest
```

## Docker images
An image is an inert, immutable, file that's essentially a snapshot of a container. 

```bash
administrator@olive-iron:~$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
minio/minio         latest              6efa3219bc21        4 hours ago         60.9MB
hello-world         latest              fce289e99eb9        6 months ago        1.84kB
```

## Create a directory to store data for MINIO
```bash
$ mkdir minio
$ cd minio
```

## Run the image
The docker run command first creates a writeable container layer over the specified image, and then starts it using the specified command.

 -d, --detach                         Run container in background and print container ID
 -e, --env list                       Set environment variables
 -v, --volume list                    Bind mount a volume

```bash
docker run -d -p 9000:9000 --name minio \
  -e "MINIO_ACCESS_KEY=JalrXUtnFEMI" \
  -e "MINIO_SECRET_KEY=AKIAIOSFODNN" \
  -v /home/administrator/minio/database:/data \
  -v /home/administrator/minio/config:/root/.minio \
  minio/minio server /data &
```

## Connect using web browser

http://olive-iron.sandbox.wwtatc.local:9000

### create bucket
Create a bucket (a directory) named `demo` and then stop the container

```bash
docker stop minio
```

### Pre-existing data

When deployed on a single drive, MinIO server lets clients access any pre-existing data in the data directory. For example, if MinIO is started with the command minio server /mnt/data, any pre-existing data in the /mnt/data directory would be accessible to the clients.


```bash
```bash
$ sudo tar xvf minio.tar --directory $HOME/minio/database/demo/
$ sudo tar xvf minio.tar
```

## Remote the container and image

```bash
$ docker ps -a
$ docker rm <containerid>
$ docker images
$ docker rmi <imageid>
```
# Author
Joel W. King   @joelwking