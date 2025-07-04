FROM almalinux:9

COPY packages /tmp/

RUN yum -y clean metadata \
    && yum install -y dnf-plugins-core \
    && yum -y install epel-release python39 python3-pip python3-devel boost-python3 jq \
    && xargs yum -y install < /tmp/packages \
    && yum -y clean all

WORKDIR /

ENV CONDA_DIR /opt/conda
RUN wget --quiet https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda.sh && \
    /bin/bash ~/miniconda.sh -b -p /opt/conda
# Put conda in path so we can use conda activate
ENV PATH=$CONDA_DIR/bin:$PATH

RUN conda init bash
RUN echo "conda activate base" >> ~/.bashrc

RUN conda install -c conda-forge python==3.11.13 -y
RUN conda install -c conda-forge jupyterlab==4.0.9 -y
RUN conda install -c conda-forge nodejs==20.9.0 -y
RUN conda install -c conda-forge jupyterhub==4.0.2 -y

ENV TINI_VERSION v0.19.0
ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini /bin/tini
RUN chmod +x /bin/tini

RUN mkdir -p /root/.ipython

COPY ./jupyterhub-singleuser.sh /usr/local/bin/jupyterhub-singleuser.sh
COPY ./jupyterhub-singleuser /usr/local/bin/jupyterhub-singleuser
COPY ./spawn.sh /root/.init/spawn.sh
RUN chmod +x /usr/local/bin/jupyterhub-singleuser.sh
RUN chmod +x /usr/local/bin/jupyterhub-singleuser && chmod +x /root/.init/spawn.sh

WORKDIR /opt/workspace
