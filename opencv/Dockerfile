FROM ubuntu:16.04

ARG OPENCV_VERSION
ARG WITH_CONTRIB

RUN apt-get update && \
apt-get install -y --no-install-recommends build-essential curl wget unzip cmake && \
rm -rf /var/lib/apt/lists/* && \
mkdir opencv && \
cd opencv && \
wget https://github.com/Itseez/opencv/archive/${OPENCV_VERSION}.zip --no-check-certificate -O opencv-${OPENCV_VERSION}.zip && \
unzip opencv-${OPENCV_VERSION}.zip && \
if [ -n "$WITH_CONTRIB" ]; then \
	wget https://github.com/Itseez/opencv_contrib/archive/${OPENCV_VERSION}.zip --no-check-certificate -O opencv_contrib-${OPENCV_VERSION}.zip; \
	unzip opencv_contrib-${OPENCV_VERSION}.zip; \
fi && \
mkdir opencv-${OPENCV_VERSION}/build && \
cd opencv-${OPENCV_VERSION}/build && \
cmake_flags="-D CMAKE_BUILD_TYPE=RELEASE \
	-D BUILD_PERF_TESTS=OFF \
	-D BUILD_TESTS=OFF \
	-D BUILD_opencv_apps=OFF \
	-D CMAKE_BUILD_TYPE=RELEASE \
	-D CMAKE_INSTALL_PREFIX=/usr/local" && \
if [ -n "$WITH_CONTRIB" ]; then \
  cmake_flags="$cmake_flags -D OPENCV_EXTRA_MODULES_PATH=../../opencv_contrib-${OPENCV_VERSION}/modules"; \
fi && \
echo $cmake_flags && \
cmake $cmake_flags .. && \
make -j $(nproc) && \
make install && \
sh -c 'echo "/usr/local/lib" > /etc/ld.so.conf.d/opencv.conf' && \
ldconfig && \
cd ../../../ && \
rm -rf opencv && \
apt-get purge -y build-essential curl wget unzip cmake && \
apt-get autoremove -y --purge