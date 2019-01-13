FROM mettainnovations/ubuntu-base:16.04

#OpenCV Binaries: mettainnovations/opencv:3.4.3-cuda10.0 dockerhub image
COPY --from=mettainnovations/opencv:3.4.3 /usr/local/bin/opencv* /usr/local/bin/
COPY --from=mettainnovations/opencv:3.4.3 /usr/local/lib/libopencv* /usr/local/lib/
COPY --from=mettainnovations/opencv:3.4.3 /usr/local/lib/pkgconfig/opencv.pc /usr/local/lib/pkgconfig/
COPY --from=mettainnovations/opencv:3.4.3 /usr/local/include/opencv/ /usr/local/include/opencv/
COPY --from=mettainnovations/opencv:3.4.3 /usr/local/include/opencv2/ /usr/local/include/opencv2/

#Dlib Binaries: mettainnovations/dlib:19.16 dockerhub image
COPY --from=mettainnovations/dlib:19.16 /usr/local/lib/libdlib* /usr/local/lib/
COPY --from=mettainnovations/dlib:19.16 /usr/local/lib/cmake/ /usr/local/lib/cmake/
COPY --from=mettainnovations/dlib:19.16 /usr/local/include/dlib/ /usr/local/include/dlib/

RUN mkdir -p /home/developer
ENV HOME /home/developer
WORKDIR /home/developer

# Download and Build DocumentDBCpp
RUN cd ~ && \
    git clone https://github.com/marcoc2/DocumentDBCpp.git DocumentDBCpp && \
    mkdir docdb.build && \
    cd docdb.build && \
    cmake CASABLANCA_INCLUDE_DIR=/usr/include/ CASABLANCA_LIBRARY=/usr/lib/ ../DocumentDBCpp && \
    make && \
    make install && \
    rm -rf ~/DocumentDBCpp

# Replace 1000 with your user / group id
RUN export uid=1000 gid=1000 && \
    echo "developer:x:${uid}:${gid}:Developer,,,:/home/developer:/bin/bash" >> /etc/passwd && \
    echo "developer:x:${uid}:" >> /etc/group && \
    mkdir -p /etc/sudoers.d && \
    echo "developer ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/developer && \
    chmod 0440 /etc/sudoers.d/developer && \
    chown ${uid}:${gid} -R /home/developer && \
    adduser developer video
USER developer
