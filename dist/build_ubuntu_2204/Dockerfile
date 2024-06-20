FROM ubuntu:22.04
ARG DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get upgrade -y 
RUN apt-get install -y build-essential git cmake libboost-all-dev libeigen3-dev libvtk9-dev libvtk9-qt-dev qtbase5-dev qt5-qmake qtbase5-dev-tools libgl1-mesa-dev
RUN mkdir /build && cd /build && git clone https://github.com/rumangerst/TSOAX.git TSOAX && cd /build/TSOAX && mkdir build && cd build && cmake -DCMAKE_BUILD_TYPE=Release .. && make
RUN mkdir /build/TSOAX/build/lib && cd /build/TSOAX/build/ && ldd tsoax_gui | awk 'NF == 4 { system("cp " $3 " lib") }'
COPY /build/TSOAX/build/ build