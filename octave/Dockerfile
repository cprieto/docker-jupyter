FROM alpine:edge
LABEL mantainer "Cristian Prieto <me@cprieto.com>"

RUN apk upgrade && apk upgrade \
  && apk --no-cache add git curl build-base sudo wget curl \
  && apk --no-cache add python3 python3-dev zeromq-dev \
  && apk --no-cache add octave ghostscript --repository http://nl.alpinelinux.org/alpine/edge/testing --allow-untrusted \
  && apk --no-cache add gnuplot --repository http://nl.alpinelinux.org/alpine/edge/community --allow-untrusted \
  && pip3 install --upgrade --no-cache-dir pip notebook ipywidgets octave_kernel \
  && adduser -D jupyter \
  && echo 'jupyter ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers \
  && mkdir -p /home/jupyter/.jupyter/custom && chown jupyter -R /home/jupyter/.jupyter \
  && curl -L -o /home/jupyter/.jupyter/custom/custom.css \
    "https://gist.githubusercontent.com/cprieto/124847d48cce56adc952664ddc9661b3/raw/3e3ac783a0f4a1a77644c5f79ca4cec4f0d4d595/custom.css" \
  && mkdir /notebooks && chown jupyter /notebooks \
  && apk del wget curl \
  && rm -rf /var/cache/apk/*
  
EXPOSE 8888

USER jupyter
WORKDIR /notebooks
VOLUME ["/notebooks"]

ENTRYPOINT ["jupyter-notebook", "--no-browser", "--ip", "0.0.0.0", "--NotebookApp.token="]
