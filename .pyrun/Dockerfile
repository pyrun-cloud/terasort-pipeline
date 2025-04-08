
# Python 3.10
FROM public.ecr.aws/docker/library/python:3.10-slim-buster

RUN apt-get update \
    # Install aws-lambda-cpp build dependencies
    && apt-get install -y \
      g++ \
      make \
      cmake \
      unzip \
      wget \
    # cleanup package lists, they are not used anymore in this image
    && rm -rf /var/lib/apt/lists/* \
    && apt-cache search linux-headers-generic

ARG FUNCTION_DIR="/function"
ARG LITHOPS

# Copy function code
RUN mkdir -p ${FUNCTION_DIR}

# Update pip
RUN pip install --upgrade --ignore-installed pip wheel six setuptools \
    && pip install --upgrade --no-cache-dir --ignore-installed \
        awslambdaric \
        boto3 \
        redis \
        httplib2 \
        requests \
        numpy \
        scipy \
        pika \
        kafka-python \
        cloudpickle \
        ps-mem \
        tblib \
        aio_pika \
        pyyaml \
        plotext \
        pandas \
        polars \
        Cython \
        pyarrow \
        click \
        "$LITHOPS"

COPY requirements.txt requirements.txt

RUN pip install --no-cache-dir --no-dependencies -r requirements.txt

# Set working directory to function root directory
WORKDIR ${FUNCTION_DIR}

# Put your dependencies here, using RUN pip install... or RUN apt install...
RUN wget https://github.com/GEizaguirre/terasort-lithops/archive/refs/heads/main.zip \
    && unzip main.zip \
    && cd terasort-lithops-main \
    && pip3 install -e . \
    && cd ..
