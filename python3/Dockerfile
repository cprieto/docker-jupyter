FROM ubuntu:rolling

RUN apt update && apt install -y \
    python3 python3-pip python3-psycopg2 python3-pymysql \
  && pip3 install --no-cache-dir \
    notebook pillow numpy scipy matplotlib plotnine pandas sqlalchemy ipywidgets \
  && useradd -d /notebooks -m jupyter \
  && rm -rf /var/lib/apt/lists/*

EXPOSE 8888

USER jupyter
WORKDIR /notebooks

VOLUME ["/notebooks"]

ENTRYPOINT ["jupyter-notebook", "--no-browser", "--ip", "0.0.0.0", "--NotebookApp.token="]
