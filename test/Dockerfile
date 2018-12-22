FROM mettainnovations/ubuntu-base:latest

COPY --from=mettainnovations/opencv:3.4.4 /usr/local/bin/opencv* /usr/local/bin/
COPY --from=mettainnovations/opencv:3.4.4 /usr/local/lib/libopencv* /usr/local/lib/
COPY --from=mettainnovations/opencv:3.4.4 /usr/local/lib/pkgconfig/opencv.pc /usr/local/lib/pkgconfig/
COPY --from=mettainnovations/opencv:3.4.4 /usr/local/include/opencv/* /usr/local/include/opencv/
COPY --from=mettainnovations/opencv:3.4.4 /usr/local/include/opencv2/* /usr/local/include/opencv2/

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
