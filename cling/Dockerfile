FROM alpine:edge
LABEL mantainer "me@cprieto.com"

ENV CC=/usr/bin/clang CXX=/usr/bin/clang++

RUN RUN apt update && apt upgrade -y \
&& apt install -y git cmake python clang clang-tools libclang-6.0-dev python-dev libzmq3-dev uuid-dev wget curl pkg-config python-pip \
&& cd /opt \
&& git clone --depth 1 --branch cling-patches http://root.cern.ch/git/llvm.git /opt/llvm \
&& git clone --depth 1 --branch cling-patches http://root.cern.ch/git/clang.git /opt/llvm/tools/clang \
&& git clone --depth 1 --branch master http://root.cern.ch/git/cling.git /opt/llvm/tools/cling \
&& mkdir /opt/llvm/builddir && cd /opt/llvm/builddir \
&& CC=/usr/bin/clang CXX=/usr/bin/clang++ cmake -DCMAKE_BUILD_TYPE=Release -DPYTHON_EXECUTABLE=$(which python) .. \
&& make --jobs=$(nproc) obj.llvm-tblgen LLVMSupport LLVMTableGen llvm-tblgen intrinsics_gen LLVMProfileData AttributeCompatFuncTableGen LLVMCore LLVMIRReader LLVMCodeGen LLVMSelectionDAG LLVMAsmPrinter LLVMGlobalISel LLVMBitReader LLVMBitWriter LLVMTransformUtils LLVMInstrumentation LLVMInstCombine LLVMScalarOpts LLVMipo LLVMVectorize LLVMObjCARCOpts LLVMLinker LLVMAnalysis LLVMLTO LLVMMC LLVMMCParser LLVMMCDisassembler LLVMObject LLVMDebugInfoCodeView LLVMTarget \
&& make --jobs=$(nproc) install \
&& rm -rf /opt/llvm \
&& mkdir /opt/tmp && cd /opt/tmp \
&& wget https://github.com/zeromq/cppzmq/archive/v4.3.0.tar.gz \
&& wget https://github.com/weidai11/cryptopp/archive/CRYPTOPP_5_6_5.tar.gz \
&& wget https://github.com/nlohmann/json/archive/v3.2.0.tar.gz \
&& wget https://github.com/QuantStack/xtl/archive/0.4.16.tar.gz \
&& tar xvf v4.3.0.tar.gz && tar xvf CRYPTOPP_5_6_5.tar.gz \
&& tar xvf v3.2.0.tar.gz && tar xvf 0.4.16.tar.gz \
&& rm v4.3.0.tar.gz && rm CRYPTOPP_5_6_5.tar.gz \
&& rm v3.2.0.tar.gz && 0.4.16.tar.gz \
&& mkdir /opt/tmp/cppzmq-4.3.0/build && cd /opt/tmp/cppzmq-4.3.0/build \
&& cmake .. && make -j $(nproc) install \
&& mkdir /opt/tmp/cryptopp-CRYPTOPP_5_6_5/build \
&& cd /opt/tmp/cryptopp-CRYPTOPP_5_6_5/build \
&& cmake .. && make -j $(nproc) install \
&& mkdir /opt/tmp/json-3.2.0/build && cd /opt/tmp/json-3.2.0/build \
&& cmake .. && make -j $(nproc) install \
&& mkdir /opt/tmp/xtl-0.4.16 && cd /opt/tmp/xtl-0.4.16/build \
&& cmake .. && make install && cd /opt/tmp \
&& git clone --depth 1 --branch master https://github.com/QuantStack/xeus.git \
&& mkdir /opt/tmp/xeus/build && cd /opt/tmp/xeus/build \
&& cmake .. && make -j $(nproc) install && cd /opt/tmp \
&& wget https://github.com/zeux/pugixml/archive/v1.9.tar.gz \
&& tar xvf v1.9.tar.gz && mkdir /opt/tmp/pugixml-1.9/build && rm v1.9.tar.gz \
&& cd /opt/tmp/pugixml-1.9/build \
&& cmake .. && make -j $(nproc) install && cd /opt/tmp \
&& wget https://github.com/jarro2783/cxxopts/archive/v2.1.1.tar.gz \
&& tar xvf v2.1.1.tar.gz && rm v2.1.1.tar.gz && mkdir /opt/tmp/cxxopts-2.1.1/build \
&& cd /opt/tmp/cxxopts-2.1.1/build cmake .. && make install \
&& pip install --no-cache-dir notebook && cd /opt/tmp \
&& git clone --depth 1 --branch master https://github.com/QuantStack/xeus-cling.git \
&& /opt/tmp/xeus-cling/build && /opt/tmp/xeus-cling/build \
&& cmake .. && make -j $(nproc) install \
&& rm -rf ${BUILDDIR} && rm -rf /opt/llvm \
&& useradd -m jupyter \
&& mkdir /notebooks && chown jupyter -R /notebooks \
&& mkdir -p /home/jupyter/.jupyter/custom && chown jupyter -R /home/jupyter/.jupyter \
&& curl -L -o /home/jupyter/.jupyter/custom/custom.css \
	"https://gist.githubusercontent.com/cprieto/124847d48cce56adc952664ddc9661b3/raw/3e3ac783a0f4a1a77644c5f79ca4cec4f0d4d595/custom.css" \
&& apt clean -y && rm -rf /var/lib/apt/lists/*

EXPOSE 8888

USER jupyter
WORKDIR /notebooks

ENV PIP_FORMAT='columns'
VOLUME ["/notebooks"]

ENTRYPOINT ["jupyter-notebook", "--no-browser", "--ip", "0.0.0.0", "--NotebookApp.token="]