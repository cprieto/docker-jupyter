# Docker images for Jupyter kernels

Custom lightweight Jupyter images for your taste!

I use Jupyter images for almost everything now, but one reason is learning languages or experimenting.

This repo contains the dockerfiles for custom learning base images for certain jupyter kernels:

 * Jupyter Python3, ~615MB ([cprieto/jupyter-python3](https://hub.docker.com/r/cprieto/jupyter-python3/)), with Python3 kernel, pandas, matplotlib, Pillow and plotnine
 * Jupyter OCaml, ~700MB ([cprieto/jupyter-ocaml](https://hub.docker.com/r/cprieto/jupyter-ocaml/)), with OCaml kernel [https://github.com/akabe/ocaml-jupyter](https://github.com/akabe/ocaml-jupyter) 
 * Jupyter Elixir, ~415MB ([cprieto/jupyter-elixir](https://hub.docker.com/r/cprieto/jupyter-elixir/)), with the IElixir kernel [https://github.com/pprzetacznik/IElixir](https://github.com/pprzetacznik/IElixir)
 * Jupyter F#, ~580MB ([cprieto/jupyter-fsharp](https://hub.docker.com/r/cprieto/jupyter-fsharp/)) with the IFSharp kernel [https://github.com/fsprojects/IfSharp](https://github.com/fsprojects/IfSharp)
 * Jupyter Javascript, ~360M ([cprieto/jupyter-javascript](https://hub.docker.com/r/cprieto/jupyter-javascript)) with the IJavascript kernel [https://github.com/n-riesco/ijavascript](https://github.com/n-riesco/ijavascript)
 * Jupyter TypeScript, ~405MB ([cprieto/jupyter-typescript](https://hub.docker.com/r/cprieto/jupyter-typescript)) with the ITypeScript kernel [https://github.com/nearbydelta/itypescript](https://github.com/nearbydelta/itypescript)
 * Jupyter Kotlin, ~750MB ([cprieto/jupyter-kotlin](https://hub.docker.com/r/cprieto/jupyter-kotlin)) with the Kotlin kernel [https://github.com/ligee/kotlin-jupyter](https://github.com/ligee/kotlin-jupyter)
 * Jupyter Erlang, ~350MB ([cprieto/jupyter-erlang](https://hub.docker.com/r/cprieto/jupyter-erlang)) with Erlang kernel [https://github.com/filmor/ierl](https://github.com/filmor/ierl)
 * Jupyter Go, ~980MB ([cprieto/jupyter-go](https://hub.docker.com/r/cprieto/jupyter-go)) with LGo kernel [https://github.com/yunabe/lgo](https://github.com/yunabe/lgo), the biggest in the group thanks to a bug in the LGO kernel when using MUSL instead of GLib

To get any of these images just `docker pull <image name>` and run it with:

`docker run -p 8888:8888 <image name>`

If you want to delete the container after done (because why should you keep it?)

`docker run -p 8888:8888 --rm <image name>`

And finally, the notebooks are saved in a local volume under `/notebooks` and you can mount that directory to a local directory, for example, if you want mount and save your notebooks in the `experiments` directory in your local path

`docker run 8888:8888 --rm -v $(pwd)/experiments:/notebooks <image name>`

Enjoy!

