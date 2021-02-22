FROM debian

RUN apt-get update && apt-get -y upgrade && apt-get install -y vim

COPY docker /

ENTRYPOINT /bin/sh

# first git clone the eval repo to /Bonus/ex01/
# docker build -t ex01 .
# docker run -it --rm ex01