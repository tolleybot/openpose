FROM 972025730931.dkr.ecr.us-east-1.amazonaws.com/opencv:opencv_ffmpeg_cuda


RUN apt-get update
RUN apt-get install -y \
 	  build-essential \
      cmake \
      git \
      lsb-release \
      libssl-dev \
      libatlas-base-dev \
      libatlas-dev \
      libboost-all-dev \
      libgflags-dev \
      libgoogle-glog-dev \
      libhdf5-dev \
      libleveldb-dev \
      liblmdb-dev \
      libopencv-dev \
      libprotobuf-dev \
      libsnappy-dev \
      lsb-release \
      protobuf-compiler \
      python-dev \
      python-numpy \
      python-pip \
      python-setuptools \
      python-scipy \
      sudo \
      wget \
      unzip \
      pkg-config \
      libkrb5-dev \
      libtool \
      autoconf \
      automake 

WORKDIR /tmp

# install zmq
RUN git clone git://github.com/jedisct1/libsodium.git 
WORKDIR /tmp/libsodium
RUN git checkout e2a30a && ./autogen.sh && ./configure && make check && make install && ldconfig

WORKDIR /tmp
RUN git clone --depth 1 git://github.com/zeromq/libzmq.git
WORKDIR /tmp/libzmq
RUN ./autogen.sh && ./configure && make
RUN make install && ldconfig

WORKDIR /tmp
# install zmq c++ bindings
RUN git clone https://github.com/zeromq/cppzmq.git
WORKDIR /tmp/cppzmq/build
RUN cmake ..
RUN make install

# install json.hpp
RUN git clone https://github.com/nlohmann/json.git
RUN cp -r ./json/include/nlohmann/ /usr/local/include/

WORKDIR /
RUN rm /tmp/* -rf

RUN git clone https://github.com/tolleybot/openpose.git
# Remove sudo for docker
RUN find ./ -name CMakeLists.txt -type f -exec sed -i 's/sudo//g' {} \;

WORKDIR /openpose/build
RUN cmake ..
RUN make -j4


WORKDIR /openpose

ENTRYPOINT ["/openpose/build/examples/tutorial_wrapper/zmq_input.bin"]