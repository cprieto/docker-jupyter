# Docker images for Jupyter kernels

Custom lightweight Jupyter images for your taste!

I use Jupyter images for almost everything now, but one reason is learning languages or experimenting.

This repo contains the dockerfiles for custom learning base images for certain jupyter kernels:

 * Jupyter Python3 ~615MB ([cprieto/jupyter-python3](https://hub.docker.com/r/cprieto/jupyter-python3/)), with Python3 kernel, pandas, matplotlib, Pillow and plotnine
 * Jupyter OCaml ~700MB ([cprieto/jupyter-ocaml](https://hub.docker.com/r/cprieto/jupyter-ocaml/)), with OCaml kernel [https://github.com/akabe/ocaml-jupyter](https://github.com/akabe/ocaml-jupyter) 
 * Jupyter Elixir ~415MB ([cprieto/jupyter-elixir](https://hub.docker.com/r/cprieto/jupyter-elixir/)), with the IElixir kernel [https://github.com/pprzetacznik/IElixir](https://github.com/pprzetacznik/IElixir)
 * Jupyter F#, ~580MB([cprieto/jupyter-fsharp](https://hub.docker.com/r/cprieto/jupyter-fsharp/) with the IFSharp kernel [https://github.com/fsprojects/IfSharp](https://github.com/fsprojects/IfSharp)

To get any of these images just `docker pull <image name>` and run it with:

`docker run -p 8888:8888 <image name>`

If you want to delete the container after done (because why should you keep it?)

`docker run -p 8888:8888 --rm <image name>`

And finally, the notebooks are saved in a local volume under `/notebooks` and you can mount that directory to a local directory, for example, if you want mount and save your notebooks in the `experiments` directory in your local path

`docker run 8888:8888 --rm -v $(pwd)/experiments:/notebooks <image name>`

Enjoy!

