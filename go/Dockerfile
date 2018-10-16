FROM debian:stable-slim
LABEL mantainer "Cristian Prieto <me@cprieto.com>"

ENV GOROOT=/usr/local/go GOPATH=/opt/go LGOPATH=/opt/lgo
ENV PATH="${GOROOT}/bin:${GOPATH}/bin:${PATH}"

RUN apt update && apt upgrade \
  && apt install -y --no-install-recommends libzmq3-dev build-essential ca-certificates \
  && apt install -y --no-install-recommends wget git build-essential pkg-config curl \
  && apt install -y --no-install-recommends python3 python3-dev python3-pip python3-setuptools \ 
  && pip3 install --upgrade --no-cache-dir pip \
  && pip install --no-cache-dir notebook \
  && mkdir -p /opt/go /opt/lgo \
  && wget https://dl.google.com/go/go1.9.7.linux-amd64.tar.gz \
  && tar -xvf go1.9.7.linux-amd64.tar.gz \
  && mv go/ /usr/local \
  && go get github.com/yunabe/lgo/cmd/lgo \
  && go get -d github.com/yunabe/lgo/cmd/lgo-internal \
  && lgo install \
  && useradd -m jupyter \
  && mkdir /notebooks && chown jupyter -R /notebooks \
  && python3 $(go env GOPATH)/src/github.com/yunabe/lgo/bin/install_kernel \
  && chown jupyter -R /opt/lgo && chown jupyter -R /opt/go \
  && mkdir -p /home/jupyter/.jupyter/custom && chown jupyter -R /home/jupyter/.jupyter \
  && curl -L -o /home/jupyter/.jupyter/custom/custom.css \
    "https://gist.githubusercontent.com/cprieto/124847d48cce56adc952664ddc9661b3/raw/3e3ac783a0f4a1a77644c5f79ca4cec4f0d4d595/custom.css" \
  && apt remove -y wget curl && apt clean \
  && rm -rf /var/lib/apt/lists/*

EXPOSE 8888
USER jupyter
WORKDIR /notebooks
VOLUME ["/notebooks"]

ENTRYPOINT ["jupyter-notebook", "--no-browser", "--ip", "0.0.0.0", "--NotebookApp.token="]
