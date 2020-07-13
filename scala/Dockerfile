FROM ubuntu

RUN apt update \
    && DEBIAN_FRONTEND=noninteractive apt install -y --no-install-recommends \
    python3 python3-pip openjdk-8-jdk curl sudo \
    && useradd -m jupyter \
    && pip3 install --no-cache-dir notebook \
    && cd /home/jupyter \
    && su - jupyter -c "curl -Lo coursier https://git.io/coursier-cli" \
    && chmod a+x coursier \
    && su - jupyter -c "./coursier launch almond -- --install" \
    && rm coursier \
    && mkdir -p /home/jupyter/.jupyter/custom && chown jupyter -R /home/jupyter/.jupyter \
    && curl -L -o /home/jupyter/.jupyter/custom/custom.css \
    "https://gist.githubusercontent.com/cprieto/124847d48cce56adc952664ddc9661b3/raw/3e3ac783a0f4a1a77644c5f79ca4cec4f0d4d595/custom.css" \
    && mkdir /notebooks \
    && chown jupyter /notebooks \
    && SUDO_FORCE_REMOVE=yes apt remove -y curl sudo \
    && rm -rf /var/lib/apt/lists/*

EXPOSE 8888
USER jupyter
WORKDIR /notebooks
VOLUME ["/notebooks"]

ENTRYPOINT ["jupyter-notebook", "--no-browser", "--ip", "0.0.0.0", "--NotebookApp.token="]