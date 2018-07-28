FROM alpine:edge
LABEL mantainer "me@cprieto.com"

RUN apk update && apk upgrade \
  && apk --no-cache add python2 python2-dev build-base sudo curl py-pip \
  && apk --no-cache add ca-certificates \
  && update-ca-certificates \
  && apk --no-cache add mono --repository http://dl-3.alpinelinux.org/alpine/edge/testing/ --allow-untrusted \
  && cert-sync /etc/ssl/cert.pem \
  && pip install --no-cache-dir notebook ipywidgets \
  && jupyter nbextension enable --py --sys-prefix widgetsnbextension \
  && adduser -D jupyter && mkdir /notebooks && chown jupyter /notebooks \
  && echo 'jupyter ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers \
  && mkdir -p /opt/ifsharp && chown jupyter -R /opt \
  && su jupyter -c "curl -L -o /opt/ifsharp.zip https://github.com/fsprojects/IfSharp/releases/download/v3.0.0-beta3/IfSharp.v3.0.0-beta3.zip" \
  && su jupyter -c "unzip -d /opt/ifsharp /opt/ifsharp.zip" \
  && su jupyter -c "mono /opt/ifsharp/ifsharp.exe" \
  && mkdir -p /home/jupyter/.jupyter/custom && chown jupyter -R /home/jupyter/.jupyter \
  && curl -L -o /home/jupyter/.jupyter/custom/custom.css \
    "https://gist.githubusercontent.com/cprieto/124847d48cce56adc952664ddc9661b3/raw/3e3ac783a0f4a1a77644c5f79ca4cec4f0d4d595/custom.css" \
  && apk del curl sudo py-pip && rm /opt/ifsharp.zip \
  && rm -rf /var/cache/apk/*

EXPOSE 8888

USER jupyter
WORKDIR /notebooks

ENV PIP_FORMAT='columns'
VOLUME ["/notebooks"]

ENTRYPOINT ["jupyter-notebook", "--no-browser", "--ip", "0.0.0.0", "--NotebookApp.token="]