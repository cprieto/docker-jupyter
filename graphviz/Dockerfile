FROM alpine:edge

LABEL mantainer="Cristian Prieto <me@cprieto.com>"

RUN apk update && apk upgrade \
  && apk --no-cache add python3 python3-dev build-base ca-certificates sudo curl graphviz \
  && update-ca-certificates \
  && pip3 install --upgrade pip \
  && pip3 install --no-cache-dir notebook ipywidgets gvmagic \
  && jupyter nbextension enable --py --sys-prefix widgetsnbextension \
  && adduser -D jupyter \
  && mkdir -p /home/jupyter/.jupyter/custom && chown jupyter -R /home/jupyter/.jupyter \
  && curl -L -o /home/jupyter/.jupyter/custom/custom.css \
  "https://gist.githubusercontent.com/cprieto/124847d48cce56adc952664ddc9661b3/raw/3e3ac783a0f4a1a77644c5f79ca4cec4f0d4d595/custom.css" \
  && mkdir /notebooks \
  && chown jupyter /notebooks \
  && echo 'jupyter ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers \
  && mkdir -p /home/jupyter/.ipython/profile_default && chown jupyter -R /home/jupyter/.ipython \
  && echo "c.IPKernelApp.extensions = ['gvmagic']" > /home/jupyter/.ipython/profile_default/ipython_kernel_config.py \
  && apk del curl sudo \
  && rm -rf /var/cache/apk/*

EXPOSE 8888

USER jupyter
WORKDIR /notebooks

VOLUME ["/notebooks"]

ENTRYPOINT ["jupyter-notebook", "--no-browser", "--ip", "0.0.0.0", "--NotebookApp.token="]
