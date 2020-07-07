#     Copyright (c) 2020 World Wide Technology, LLC
#     All rights reserved.
#
#     author: joel.king@wwt.com (@joelwking)
#     written:  7 July 2020
#
#     usage:
#
FROM ubuntu:18.04

MAINTAINER @joelwking

LABEL version="1.0" description="Linter for Ansible Playbooks"
#
# Install software
#
RUN apt-get update && \
    apt-get install -y software-properties-common  && \
    DEBIAN_FRONTEND=noninteractive \
    apt-get install git -y && \
    apt-get install python-pip -y && \
    apt-get install python-dev -y  && \
    pip install pyopenssl  && \
    pip install pydevd  && \
    pip install ansible-lint  && \
    apt-add-repository --yes --update ppa:ansible/ansible  && \
    apt-get install ansible -y

#
WORKDIR /app
CMD [ "ansible-playbook", "--version" ]
