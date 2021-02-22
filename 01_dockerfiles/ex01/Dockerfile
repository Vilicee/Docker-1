FROM debian

MAINTAINER wille wvaara@hive.fi

ENV TS3SERVER_LICENSE=accept

WORKDIR /home/teamspeak

RUN apt-get update && apt-get -y upgrade && apt-get install -y wget bzip2 && \
	wget https://files.teamspeak-services.com/releases/server/3.12.1/teamspeak3-server_linux_amd64-3.12.1.tar.bz2 && \
	tar -xvf teamspeak3-server_linux_amd64-3.12.1.tar.bz2

EXPOSE 9987/udp 10011 30033

WORKDIR teamspeak3-server_linux_amd64

# to open the ports needed for voice, filetransfer and serverquery

ENTRYPOINT sh ts3server_minimal_runscript.sh
# docker build -t ex01 .
# docker run -dit --name my_teamspeak --rm -p 9987:9987/udp -p 10011:10011 -p 30033:30033 ex01
# docker logs my_teamspeak to get the token