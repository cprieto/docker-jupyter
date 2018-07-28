FROM alpine:edge

LABEL mantainer "me@cprieto.com"

ARG OPAM_VERSION=1.2.2
ARG OCAML_VERSION=4.07.0
ARG HOME=/home/jupyter

RUN mkdir /lib64 && ln -s /lib/libc.musl-x86_64.so.1 /lib64/ld-linux-x86-64.so.2 \
  && apk --no-cache update && apk --no-cache upgrade \
  && apk add --no-cache --upgrade zlib gmp zeromq libx11 aspcud python libev libffi \
  && apk add --no-cache --upgrade --virtual=.build-deps bash musl-dev make patch gcc build-base libx11-dev curl m4 perl libev-dev zlib-dev gmp-dev zeromq-dev libffi-dev python-dev py2-pip \
  && adduser -h $HOME -s /bin/sh -D jupyter \
  && echo 'opam ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers \
  && curl -L -o /usr/bin/opam \
    "https://github.com/ocaml/opam/releases/download/$OPAM_VERSION/opam-$OPAM_VERSION-$(uname -m)-$(uname -s)" \
	&& chmod 755 /usr/bin/opam \
	&& su jupyter -c "opam init -a -y --comp $OCAML_VERSION" \
	&& find $HOME/.opam -regex '.*\.\(cmt\|cmti\|annot\|byte\)' -delete \
  && rm -rf $HOME/.opam/archives $HOME/.opam/repo/default/archives \
    $HOME/.opam/$OCAML_VERSION/man $HOME/.opam/$OCAML_VERSION/build \
  && pip install --no-cache-dir notebook ipywidgets \
  && su - jupyter -c "opam update" \
  && su - jupyter -c "opam install -y 'merlin>=3.0.0' ocaml-migrate-parsetree jupyter" \
  && su - jupyter -c "echo $(opam config var share)/jupyter > ~/location.tmp" \
  && jupyter nbextension enable --py --sys-prefix widgetsnbextension \
  && jupyter kernelspec install --user --name ocaml-jupyter $(cat $HOME/location.tmp) && rm $HOME/location.tmp \
  && mkdir -p $HOME/.jupyter && mkdir -p $HOME/.jupyter/custom && mkdir -p $HOME/.jupyter/nbconfig \
  && curl -L -o $HOME/.jupyter/custom/custom.js \
    "https://raw.githubusercontent.com/akabe/docker-ocaml-jupyter/master/.jupyter/custom/custom.js" \
  && mkdir -p $HOME/.jupyter/custom && chown jupyter -R $HOME/.jupyter \
  && curl -L -o $HOME/.jupyter/custom/custom.css \
    "https://gist.githubusercontent.com/cprieto/124847d48cce56adc952664ddc9661b3/raw/3e3ac783a0f4a1a77644c5f79ca4cec4f0d4d595/custom.css" \
  && curl -L -o $HOME/.jupyter/nbconfig/notebook.json \
    "https://raw.githubusercontent.com/akabe/docker-ocaml-jupyter/master/.jupyter/nbconfig/notebook.json" \
  && curl -L -o $HOME/.ocamlinit \
    "https://raw.githubusercontent.com/akabe/docker-ocaml-jupyter/master/.ocamlinit" \
  && mkdir /notebooks && chown -R jupyter $HOME && chown -R jupyter /notebooks \
  && apk del .build-deps && rm -rf /var/cache/apk/* \
  && if [ ! -f /usr/lib/libX11.so ]; then ln -s /usr/lib/libX11.so.6 /usr/lib/libX11.so; fi

EXPOSE 8888
USER jupyter
WORKDIR /notebooks

VOLUME ["/notebooks"]
ENTRYPOINT ["opam", "config", "exec", "--", "jupyter-notebook", "--no-browser", "--ip", "0.0.0.0", "--NotebookApp.token="]
