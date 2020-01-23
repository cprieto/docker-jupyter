FROM alpine:edge
LABEL mantainer "Cristian Prieto <me@cprieto.com>"

ENV PATH="/opt/almond:${PATH}" SCALA_VERSION=2.13.0 ALMOND_VERSION=0.9.1

RUN apk upgrade && apk upgrade \
  && apk add --no-cache openjdk8 python3 zeromq-dev python3-dev build-base \
  && apk add --no-cache git sudo bash curl \
  && adduser -D jupyter \
  && mkdir /opt/almond && cd /opt/almond \
  && curl -Lo coursier https://git.io/coursier-cli \
  && chmod a+x coursier && chown jupyter -R /opt/almond \
  && ./coursier bootstrap \
  -r jitpack \
  -i user -I user:sh.almond:scala-kernel-api_$SCALA_VERSION:$ALMOND_VERSION \
  sh.almond:scala-kernel_$SCALA_VERSION:$ALMOND_VERSION \
  -o almond \
  && chown jupyter -R /opt/almond \
  && pip3 install --upgrade pip \
  && pip3 install --no-cache-dir notebook ipywidgets \
  && jupyter nbextension enable --py --sys-prefix widgetsnbextension \
  && su - jupyter -c "cd /opt/almond && ./almond --install" \
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
VOLUME ["/notebooks"]

ENTRYPOINT ["jupyter-notebook", "--no-browser", "--ip", "0.0.0.0", "--NotebookApp.token="]
