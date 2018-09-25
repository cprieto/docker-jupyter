FROM alpine:edge
LABEL mantainer "Cristian Prieto <me@cprieto.com>"

RUN apk upgrade && apk upgrade \
  && apk add --no-cache openjdk8 python3 python3-dev build-base \
  && apk add --no-cache git ca-certificates sudo bash curl \
  && update-ca-certificates \
  && mkdir -p /opt/iktl \
  && git clone https://github.com/ligee/kotlin-jupyter.git /opt/iktl \
  && rm -rf /opt/iktl/.git \
  && adduser -D jupyter \
  && chown jupyter -R /opt/iktl \
  && pip3 install --upgrade pip \
  && pip3 install --no-cache-dir notebook ipywidgets \
  && jupyter nbextension enable --py --sys-prefix widgetsnbextension \
  && su - jupyter -c "cd /opt/iktl && ./gradlew install" \
  && mkdir -p /home/jupyter/.jupyter/custom && chown jupyter -R /home/jupyter/.jupyter \
  && curl -L -o /home/jupyter/.jupyter/custom/custom.css \
  "https://gist.githubusercontent.com/cprieto/124847d48cce56adc952664ddc9661b3/raw/3e3ac783a0f4a1a77644c5f79ca4cec4f0d4d595/custom.css" \
  && mkdir /notebooks \
  && chown jupyter /notebooks \
  && apk del git sudo bash curl \
  && rm -rf /var/cache/apk/*

EXPOSE 8888

USER jupyter
WORKDIR /notebooks

ENV PIP_FORMAT='columns'
VOLUME ["/notebooks"]

ENTRYPOINT ["jupyter-notebook", "--no-browser", "--ip", "0.0.0.0", "--NotebookApp.token="]
