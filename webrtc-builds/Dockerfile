FROM ubuntu:xenial

WORKDIR /

RUN apt-get update -y

# Deps
RUN apt-get install -y vim python-pip polipo wget git gnupg flex bison gperf build-essential zip curl subversion pkg-config libglib2.0-dev libgtk2.0-dev libxtst-dev libxss-dev libpci-dev libdbus-1-dev libgconf2-dev libgnome-keyring-dev libnss3-dev lsb-release sudo locales libssl-dev openssl cmake lbzip2 libavcodec-dev libavdevice-dev libavfilter-dev libavformat-dev libswresample-dev libpostproc-dev
RUN pip install shadowsocks

# libsodium
#RUN wget https://download.libsodium.org/libsodium/releases/libsodium-1.0.16.tar.gz
#RUN tar zxf libsodium-1.0.16.tar.gz
#WORKDIR /libsodium-1.0.16
#RUN ./configure && make && make install
#RUN echo /usr/local/lib > /etc/ld.so.conf.d/usr_local_lib.conf && ldconfig

WORKDIR /

# install webrtc deps
RUN curl https://chromium.googlesource.com/chromium/src/+/master/build/install-build-deps-android.sh?format=TEXT | base64 -d > install-build-deps-android.sh
RUN curl https://chromium.googlesource.com/chromium/src/+/master/build/install-build-deps.sh?format=TEXT | base64 -d > install-build-deps.sh
RUN chmod +x ./install-build-deps.sh
RUN chmod +x ./install-build-deps-android.sh
RUN echo ttf-mscorefonts-installer msttcorefonts/accepted-mscorefonts-eula select true | debconf-set-selections
RUN ./install-build-deps-android.sh

# depot tools
WORKDIR /
RUN git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git

RUN echo 'export PATH=$PATH:/depot_tools' >> /root/.bashrc

COPY ["resource/pkgconfig/libwebrtc_full.pc.in", "/resource/pkgconfig/libwebrtc_full.pc.in"]

COPY ["build.sh", "/build.sh"]
COPY ["util.sh", "/util.sh"]

RUN chmod +x /build.sh
RUN chmod +x /util.sh
RUN /build.sh

#CMD out/src/out/linux-x64/Debug/peerconnection_server&
#CMD out/src/out/linux-x64/Debug/stunserver&
#CMD out/src/out/linux-x64/Debug/turnserver&

EXPOSE 8888
EXPOSE 8080
EXPOSE 8089
EXPOSE 3478
EXPOSE 3033



