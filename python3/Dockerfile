FROM alpine:edge
LABEL mantainer "me@cprieto.com"

RUN apk update && apk upgrade \
    && apk --no-cache add python3 python3-dev build-base freetype-dev sudo \
    && apk --no-cache add libxml2 libxml2-dev libxslt libxslt-dev curl \
    && apk --no-cache add jpeg jpeg-dev zlib zlib-dev libffi libffi-dev \
    && apk --no-cache add lapack lapack-dev gfortran ca-certificates \
    && apk --no-cache add py-geos gdal-dev --repository http://dl-3.alpinelinux.org/alpine/edge/testing \
    && update-ca-certificates \
    && pip3 install --upgrade pip \
    && pip3 install --no-cache-dir notebook numpy \
    && pip3 install --no-cache-dir matplotlib Pillow ipywidgets \
    && pip3 install --no-cache-dir pandas scipy plotnine \
    && jupyter nbextension enable --py --sys-prefix widgetsnbextension \
    && adduser -D jupyter \
    && mkdir -p /home/jupyter/.jupyter/custom && chown jupyter -R /home/jupyter/.jupyter \
    && curl -L -o /home/jupyter/.jupyter/custom/custom.css \
    "https://gist.githubusercontent.com/cprieto/124847d48cce56adc952664ddc9661b3/raw/3e3ac783a0f4a1a77644c5f79ca4cec4f0d4d595/custom.css" \
    && mkdir /notebooks \
    && chown jupyter /notebooks \
    && echo 'jupyter ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers \
    && apk del curl \
    && rm -rf /var/cache/apk/*

EXPOSE 8888

USER jupyter
WORKDIR /notebooks

ENV PIP_FORMAT='columns'
VOLUME ["/notebooks"]

ENTRYPOINT ["jupyter-notebook", "--no-browser", "--ip", "0.0.0.0", "--NotebookApp.token="]
