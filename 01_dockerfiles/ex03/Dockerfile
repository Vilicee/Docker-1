FROM ubuntu:16.04

MAINTAINER wvaara <wvaara@hive.fi>

ARG IP="0.0.0.0"

RUN apt-get update && \
	apt-get -y upgrade && \
	apt-get install -y wget && \
	apt-get install -y openssh-server && \
	apt-get install -y ca-certificates && \
	apt-get install -y postfix && \
	wget https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh && \
	chmod 777 script.deb.sh && \
	./script.deb.sh && \
	apt-get install -y gitlab-ce && \
	apt-get update && \
	apt-get install -y tzdata && \
	apt-get clean && \
	rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
	sed -i "s/# grafana\['enable'\] = true/grafana['enable'] = false/g" /etc/gitlab/gitlab.rb && \
	echo "UseDNS no" >> /etc/ssh/sshd_config && \
	mkdir -p /etc/gitlab/ssl && \
    chmod +x /etc/gitlab/ssl && \
    openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
    -subj /C=FI/ST=Helsinki/L=Helsinki/O=42/OU=0/CN=${IP}/emailAddress=wvaara@hive.fi \
    -keyout /etc/gitlab/ssl/selfsigned.key -out /etc/gitlab/ssl/selfsigned.crt && \
    echo "external_url \"https://${IP}\"" >> /etc/gitlab/gitlab.rb && \
    echo "nginx['redirect_http_to_https'] = true" >> /etc/gitlab/gitlab.rb && \
    echo "nginx['ssl_certificate'] = \"/etc/gitlab/ssl/selfsigned.crt\"" >> /etc/gitlab/gitlab.rb && \
    echo "nginx['ssl_certificate_key'] = \"/etc/gitlab/ssl/selfsigned.key\"" >> /etc/gitlab/gitlab.rb && \
    echo "gitlab_rails['gitlab_shell_ssh_port'] = 8022" >> /etc/gitlab/gitlab.rb	

EXPOSE 443 80 22

ENTRYPOINT service ssh start && (/opt/gitlab/embedded/bin/runsvdir-start &) && gitlab-ctl reconfigure && tail -f /dev/null

# TO BUILD
# docker build -t ex03 --build-arg IP=$(docker-machine ip Char) .
# TO RUN
# docker run -it --rm -p 80:80 -p 8022:22 -p 443:443 --privileged ex03
# HTTPS
# GIT_SSL_NO_VERIFY=true git clone https://$(docker-machine ip Char)
# GIT_SSL_NO_VERIFY=true git push origin master
# SSH
# ADD YOUR KEY
# then just normal git clone commit push etc.