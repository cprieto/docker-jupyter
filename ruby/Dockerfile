FROM alpine:edge
LABEL mantainer "me@cprieto.com"

RUN apk update && apk upgrade \
    && apk --no-cache add python3 python3-dev build-base sudo \
    && apk --no-cache add curl py3-zmq \
    && apk --no-cache add ruby ruby-dev czmq-dev libffi libffi-dev ruby-rdoc \
    && update-ca-certificates \
    && pip3 install --upgrade pip \
    && pip3 install --disable-pip-version-check --no-cache-dir notebook ipywidgets \
    && jupyter nbextension enable --py --sys-prefix widgetsnbextension \
    && adduser -D jupyter \
    && gem install cztop \
    && gem install iruby --pre \
    && su - jupyter -c "iruby register --force" \
    && mkdir -p /home/jupyter/.jupyter/custom && chown jupyter -R /home/jupyter/.jupyter \
    && curl -L -o /home/jupyter/.jupyter/custom/custom.css \
    "https://gist.githubusercontent.com/cprieto/124847d48cce56adc952664ddc9661b3/raw/3e3ac783a0f4a1a77644c5f79ca4cec4f0d4d595/custom.css" \
    && mkdir /notebooks \
    && chown jupyter /notebooks \
    && echo 'jupyter ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers \
    && apk del curl git \
    && rm -rf /var/cache/apk/*

EXPOSE 8888

USER jupyter
WORKDIR /notebooks

ENV PIP_FORMAT='columns'
VOLUME ["/notebooks"]

ENTRYPOINT ["jupyter-notebook", "--no-browser", "--ip", "0.0.0.0", "--NotebookApp.token="]
