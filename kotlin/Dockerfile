FROM ubuntu
LABEL mantainer "Cristian Prieto <me@cprieto.com>"

RUN apt update \
  && DEBIAN_FRONTEND=noninteractive apt install -y --no-install-recommends \
    openjdk-11-jdk python3 python3-pip curl \
  && pip3 install --no-cache-dir notebook kotlin-jupyter-kernel \
  && useradd -m jupyter \
  && mkdir -p /home/jupyter/.jupyter/custom && chown jupyter -R /home/jupyter/.jupyter \
  && curl -L -o /home/jupyter/.jupyter/custom/custom.css \
  "https://gist.githubusercontent.com/cprieto/124847d48cce56adc952664ddc9661b3/raw/3e3ac783a0f4a1a77644c5f79ca4cec4f0d4d595/custom.css" \
  && mkdir /notebooks \
  && apt remove -y curl \
  && chown jupyter /notebooks \
  && rm -rf /var/lib/apt/lists/*

EXPOSE 8888

USER jupyter
WORKDIR /notebooks
VOLUME ["/notebooks"]

ENTRYPOINT ["jupyter-notebook", "--no-browser", "--ip", "0.0.0.0", "--NotebookApp.token="]
