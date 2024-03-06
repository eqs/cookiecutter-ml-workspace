{%- if cookiecutter.use_cuda -%}
FROM nvidia/cuda:12.3.1-runtime-ubuntu22.04
{%- else -%}
FROM ubuntu:22.04
{%- endif %}

ENV TZ=Asia/Tokyo
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# Update packages and install dependencies
RUN apt-get -y update --fix-missing && apt-get -y upgrade
RUN apt-get -y install sudo wget curl bzip2 git vim make cmake unzip python3
RUN apt-get -y install ca-cacert build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev \
    libsqlite3-dev llvm libncurses5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev

# settings
ARG user_name=ubuntu
ARG user_id=1000
ARG group_name=ubuntu
ARG group_id=1000

# create user
RUN groupadd -g ${group_id} ${group_name}
RUN useradd -u ${user_id} -g ${group_id} -d /home/${user_name} \
    --create-home --shell /bin/bash ${user_name}
RUN echo "${user_name} ALL=(ALL) NOPASSWD:ALL \n" >> /etc/sudoers
RUN echo "Defaults env_keep += \"PATH\" \n" >> /etc/sudoers
RUN chown -R ${user_name}:${group_name} /home/${user_name}

# user settings
ENV HOME /home/${user_name}
ENV LANG en_US.UTF-8

USER ${user_name}

WORKDIR /work

{%- if cookiecutter.use_cuda %}
ENV LD_LIBRARY_PATH /usr/local/cuda-12.3/lib64:/usr/local/cuda-12.3/extras/CUPTI/lib64:$LD_LIBRARY_PATH
RUN echo 'export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/lib' >> ~/.bashrc
{%- endif %}

ENV PYTHON_VERSION 3.11.8

# Install Python
RUN git clone https://github.com/pyenv/pyenv.git ~/.pyenv && \
    echo 'eval "$(pyenv init -)"' >> ~/.bashrc
ENV PATH ~/.pyenv/bin:$PATH
RUN /bin/bash -c "source ~/.bashrc \
    && LANG=C.UTF-8 \
    && LC_ALL=C.UTF-8 \
    && LC_CTYPE=C.UTF-8 \
    && pyenv install ${PYTHON_VERSION} \
    && pyenv global ${PYTHON_VERSION}"

ADD requirements.txt /work/
RUN /bin/bash -c "eval \"\$(pyenv init -)\" \
    && python -m pip install -r requirements.txt"

ENV LC_ALL C.UTF-8
ENV LANG C.UTF-8

CMD ["/bin/bash"]
