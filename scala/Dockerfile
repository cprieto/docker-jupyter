FROM alpine:edge
LABEL mantainer "Cristian Prieto <me@cprieto.com>"

ENV PATH="/opt/almond:${PATH}"

RUN apk upgrade && apk upgrade \
  && apk add --no-cache openjdk8 python3 python3-dev build-base \
  && apk add --no-cache git ca-certificates sudo bash curl \
  && update-ca-certificates \
  && mkdir /opt/almond && cd /opt/almond \
  && curl -L -o coursier https://git.io/coursier \
  && chmod a+x coursier \
  && adduser -D jupyter \
  && chown jupyter -R /opt/almond \
  && su - jupyter -c 'cd /opt/almond && ./coursier bootstrap -i user -I user:sh.almond:scala-kernel-api_2.12.7:0.1.10 sh.almond:scala-kernel_2.12.7:0.1.10 -o almond' \
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