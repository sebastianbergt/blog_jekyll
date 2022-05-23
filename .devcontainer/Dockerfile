FROM ubuntu:20.04
ENV DEBIAN_FRONTEND=noninteractive 
RUN apt-get update
RUN apt-get install -y git vim jekyll
RUN apt-get clean -y
RUN rm -rf /var/lib/apt/lists/*

CMD jekyll build blog