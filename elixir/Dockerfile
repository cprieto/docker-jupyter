FROM alpine:latest
LABEL mantainer "Cristian Prieto <me@cprieto.com>"

ENV MIX_HOME=/opt/mix PATH=/opt/mix:${PATH}

RUN apk upgrade && apk upgrade \
  && apk --no-cache add git curl build-base sudo wget curl \
  && apk --no-cache add python2 python2-dev py-pip \
  && apk --no-cache add zeromq-dev erlang-dev elixir erlang-crypto \
  && pip install --upgrade --no-cache-dir notebook ipywidgets \
  && adduser -D jupyter \
  && echo 'jupyter ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers \
  && mkdir -p /opt && chown jupyter:jupyter -R /opt \
  && su jupyter -c 'mix local.rebar --force' && su jupyter -c 'mix local.hex --force' \
  && su jupyter -c 'git clone https://github.com/pprzetacznik/IElixir.git /opt/ielixir' \
  && cd /opt/ielixir && su jupyter -c 'mix deps.get && MIX_ENV=prod mix compile' \
  && mkdir -p /home/jupyter/.jupyter/custom && chown jupyter -R /home/jupyter/.jupyter \
  && curl -L -o /home/jupyter/.jupyter/custom/custom.css \
    "https://gist.githubusercontent.com/cprieto/124847d48cce56adc952664ddc9661b3/raw/3e3ac783a0f4a1a77644c5f79ca4cec4f0d4d595/custom.css" \
  && su jupyter -c './install_script.sh' \
  && mkdir /notebooks && chown jupyter /notebooks \
  && apk del wget curl \
  && rm -rf /opt/ielixir/.git \
  && rm -rf /var/cache/apk/*
  
EXPOSE 8888

USER jupyter
WORKDIR /notebooks
VOLUME ["/notebooks"]

ENTRYPOINT ["jupyter-notebook", "--no-browser", "--ip", "0.0.0.0", "--NotebookApp.token="]
