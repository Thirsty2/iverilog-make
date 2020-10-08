ARG IMAGE="iverilog"
# Note that some commands such as 'apk add' won't work in arbitrary images.  For instance,
# 'apk add' works fine under alpine, but not under Ubuntu (at least not by default.)
# In other words, the following 'apk add' sections assume an alpine based image or a derivitive

FROM $IMAGE as base

USER root

RUN apk add --no-cache \
        make

FROM base as release-candidate

RUN adduser --disabled-password ic
USER ic
WORKDIR /home/ic

ENTRYPOINT [ "make" ]

FROM release-candidate as iverilog-make

# Below are some sample commands to build and run a docker images.
# 
# docker build  -f Dockerfile.entrypoint-make . -t iverilog-make
#
# In a directory containing a Makefile
# docker run -v $(pwd):/home/ic iverilog-make
# or call it with an optional makefile target like clean:
# docker run -v $(pwd):/home/ic iverilog-make clean

