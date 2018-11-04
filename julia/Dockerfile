FROM debian:testing-slim
LABEL mantainer "Cristian Prieto <me@cprieto.com>"

RUN apt update && apt upgrade -y \
  && apt install julia curl -y \
  && apt install -y --no-install-recommends python3 python3-dev python3-pip python3-setuptools \
  && pip3 install --upgrade --no-cache-dir pip \
  && pip install --no-cache-dir notebook \
  && useradd -m jupyter \
  && mkdir /notebooks && chown jupyter -R /notebooks \
  && su - jupyter -c "julia -e 'using Pkg; Pkg.add(\"IJulia\")'" \
  && mkdir -p /home/jupyter/.jupyter/custom && chown jupyter -R /home/jupyter/.jupyter \
  && curl -L -o /home/jupyter/.jupyter/custom/custom.css \
    "https://gist.githubusercontent.com/cprieto/124847d48cce56adc952664ddc9661b3/raw/3e3ac783a0f4a1a77644c5f79ca4cec4f0d4d595/custom.css" \
  && apt remove -y curl \
  && apt clean -y && apt autoremove -y \
  && rm -rf /var/lib/apt/lists/*

EXPOSE 8888
USER jupyter
WORKDIR /notebooks
VOLUME ["/notebooks"]

ENTRYPOINT ["jupyter-notebook", "--no-browser", "--ip", "0.0.0.0", "--NotebookApp.token="]
