ARG MICRO_IMAGE_DIR=/ubi-micro-img

# BASE image using UBI 9 micro where the
# application and requirements will be installed
FROM registry.access.redhat.com/ubi9-micro:9.4-15 AS base

# BUILD image using UBI 9 where the dependencies that
# require installing with a package manager will be installed
FROM registry.access.redhat.com/ubi9:9.4-1214.1726694543 AS build
ARG MICRO_IMAGE_DIR

# Copy the BASE image into the BUILD image
RUN mkdir ${MICRO_IMAGE_DIR}
COPY --from=base / ${MICRO_IMAGE_DIR}

# Install Python inside the BASE image
RUN dnf install --installroot ${MICRO_IMAGE_DIR} --nodocs -y \
    python3.12 \
    python3.12-devel \
    libstdc++ &&\
    dnf upgrade --installroot ${MICRO_IMAGE_DIR} --nodocs -y && \
    dnf clean all --installroot ${MICRO_IMAGE_DIR}

# APP image from `scratch` which will be the final image
# and remaining application requirements will be installed
FROM scratch AS app
ARG MICRO_IMAGE_DIR
COPY --from=build ${MICRO_IMAGE_DIR}/ .

# create symlinks for python
RUN ln -s /usr/bin/python3.11 /usr/bin/python

# Create project dir
WORKDIR /usr/src/app

# set environment variables
ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1

# Install pip
RUN python3.12 -m ensurepip --upgrade
# Install dependencies and update then uninstall pip (not needed in final image)
RUN python3.12 -m pip install openai pyyaml python-dotenv requests flask --no-cache-dir --upgrade && \
    python3.12 -m pip uninstall -y pip

COPY maestro ./src
COPY tmp/entrypoint_api.sh .
COPY tmp/api.py .
COPY tmp/maestro.html ./templates/index.html
COPY tmp/agents.yaml ./src/agents.yaml
COPY tmp/workflow.yaml ./src/workflow.yaml
COPY tmp/workflow.yaml ./static/workflow.yaml
COPY tmp/agents.yaml ./static/agents.yaml

RUN chown -R 1000:100 /usr/src/app &&\
    mkdir /usr/src/app/media && chown 1000:100 /usr/src/app/media

EXPOSE 5000
USER 1000:100
ENTRYPOINT ["/usr/src/app/entrypoint_api.sh"]
