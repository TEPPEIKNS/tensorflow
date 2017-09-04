FROM ubuntu:16.04

#Fork from tensorflow/tensorflow
MAINTAINER Teppei Konishi <teppei.konishi.xt@gmail.com>

# Pick up some TF dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
        build-essential \
        curl \
        libfreetype6-dev \
        libpng12-dev \
        libzmq3-dev \
        pkg-config \
        python3 \
        python3-dev \
        rsync \
        software-properties-common \
        unzip \
        mecab \
        libmecab-dev \
        mecab-ipadic \
        mecab-ipadic-utf8 \
        python-mecab \
        language-pack-ja-base \
        language-pack-ja \
        ibus-mozc \
        vim \
        && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN curl -O https://bootstrap.pypa.io/get-pip.py && \
    python3 get-pip.py && \
    rm get-pip.py

RUN pip3 install \
        Pillow \
        h5py \
        ipykernel \
        jupyter \
        matplotlib \
        numpy \
        pandas \
        scipy \
        sklearn \
        slackbot \
        mecab-python3 \
        bs4 \
        && \
    python3 -m ipykernel.kernelspec

RUN update-locale LANG=ja_JP.UTF-8 LANGUAGE="ja_JP:ja"
RUN . /etc/default/locale
RUN export LANG=ja_JP.UTF-8

# --- DO NOT EDIT OR DELETE BETWEEN THE LINES --- #
# These lines will be edited automatically by parameterized_docker_build.sh. #
# COPY _PIP_FILE_ /
# RUN pip --no-cache-dir install /_PIP_FILE_
# RUN rm -f /_PIP_FILE_

# Install TensorFlow CPU version from central repo
ENV TENSORFLOW_VERSION 0.13.0
RUN pip3 --no-cache-dir install tensorflow
#/http://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-${TENSORFLOW_VERSION}-cp36-cp36m-linux_x86_64.whl
# --- ~ DO NOT EDIT OR DELETE BETWEEN THE LINES --- #

#RUN ln -s /usr/bin/python3 /usr/bin/python

# Set up our notebook config.
COPY jupyter_notebook_config.py /root/.jupyter/

# Copy sample notebooks.
COPY notebooks /notebooks

# Jupyter has issues with being run directly:
#   https://github.com/ipython/ipython/issues/7062
# We just add a little wrapper script.
COPY run_jupyter.sh /

# TensorBoard
EXPOSE 6006
# IPython
EXPOSE 8888

WORKDIR "/notebooks"

ENTRYPOINT ["/bin/bash"]
#CMD ["/run_jupyter.sh", "--allow-root"]
