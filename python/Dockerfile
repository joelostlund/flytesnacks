FROM ubuntu:bionic

WORKDIR /app
ENV VENV /app/venv
ENV LANG C.UTF-8
ENV LC_ALL C.UTF-8
ENV PYTHONPATH /app

# Install Python3 and other basics
RUN apt-get update && apt-get install -y python3.6 python3.6-venv make build-essential libssl-dev python3-venv python3-pip curl

# opencv support
RUN apt-get install -y libsm6 libxext6 libxrender-dev

# Google Cloud SDK & Configure gcloud
RUN curl -sSL https://sdk.cloud.google.com | bash
ENV PATH="$PATH:/root/google-cloud-sdk/bin"

# Run this before dealing with our own virtualenv. The AWS CLI uses its own virtual environment. 
# NOTE: We are installing multi-cloud Download managers to make the examples completely portable. In your repos, please install only the utilities for cloud providers that you use.
RUN pip3 install awscli

# Set up a virtual environment to use with our workflows
RUN python3.6 -m venv ${VENV}
RUN ${VENV}/bin/pip install wheel

COPY ./requirements.txt .
RUN ${VENV}/bin/pip install -r requirements.txt

# This is a script that enables a virtualenv, copy it to a better location
RUN cp ${VENV}/bin/flytekit_venv /opt/

# Copy the rest of the code
COPY . .

# Set this environment variable. It will be used by the flytekit SDK during the registration/compilation steps
ARG tag
ENV FLYTE_INTERNAL_IMAGE=$tag

# Enable the virtualenv for this image. Note this relies on the VENV variable we've set in this image.
ENTRYPOINT ["/opt/flytekit_venv"]
