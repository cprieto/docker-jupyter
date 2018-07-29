FROM alpine:edge

LABEL mantainer "Cristian Prieto <me@cprieto.com>"

RUN apk update && apk upgrade \
  && apk add --no-cache python python-dev build-base sudo \
  && apk add --no-cache nodejs nodejs-npm libzmq \
  && apk add --virtual .build-deps --no-cache wget git py2-pip nodejs-dev zeromq-dev curl \
  && pip install notebook ipywidgets \
  && adduser -D jupyter \
  && echo 'jupyter ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers \
  && mkdir -p /opt/ijs && chown jupyter:jupyter -R /opt \
  && npm install --unsafe-perm -g itypescript \
  && cd /home/jupyter && su jupyter -c 'its --ts-install=local' \
  && jupyter nbextension enable --py --sys-prefix widgetsnbextension \
  && mkdir -p /home/jupyter/.jupyter/custom && chown jupyter -R /home/jupyter/.jupyter \
  && curl -L -o /home/jupyter/.jupyter/custom/custom.css \
    "https://gist.githubusercontent.com/cprieto/124847d48cce56adc952664ddc9661b3/raw/3e3ac783a0f4a1a77644c5f79ca4cec4f0d4d595/custom.css" \
  && mkdir /notebooks && chown jupyter:jupyter /notebooks \
  && apk del .build-deps \
  && rm -rf /var/cache/apk/*

EXPOSE 8888

USER jupyter
WORKDIR /notebooks

VOLUME ["/notebooks"]

ENTRYPOINT ["jupyter-notebook", "--no-browser", "--ip", "0.0.0.0", "--NotebookApp.token="]

