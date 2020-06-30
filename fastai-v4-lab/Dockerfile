FROM nvidia/cuda:10.1-base-ubuntu18.04
# See http://bugs.python.org/issue19846
ENV LANG C.UTF-8
LABEL com.nvidia.volumes.needed="nvidia_driver"

RUN echo "deb http://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1604/x86_64 /" > /etc/apt/sources.list.d/nvidia-ml.list

RUN apt-get update && apt-get install -y --no-install-recommends \
         build-essential \
         apt-utils \
         cmake \
         git \
         curl \
         vim \
         ca-certificates \
         python-qt4 \
         libjpeg-dev \
         zip \
         unzip \
         libpng-dev &&\
     rm -rf /var/lib/apt/lists/*

ENV LD_LIBRARY_PATH /usr/local/nvidia/lib:/usr/local/nvidia/lib64
ENV PYTHON_VERSION=3.7

RUN curl -L https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh -o miniconda.sh \
  && bash miniconda.sh -b -p /opt/conda \
  && rm -f miniconda.sh

ENV PATH=$PATH:/opt/conda/bin/

RUN /opt/conda/bin/conda install conda-build

ENV USER fastai

# Create Enviroment
COPY enviroment.yaml /enviroment.yaml
# RUN /opt/conda/bin/conda env create -f enviroment.yaml
RUN conda env update --name base --file enviroment.yaml

WORKDIR /notebooks

# Activate Source
# CMD source activate base
CMD source ~/.bashrc

RUN chmod -R a+w /notebooks
WORKDIR /notebooks

COPY config.yml /root/.fastai/config.yml
COPY run.sh /run.sh

CMD ["/run.sh"]
