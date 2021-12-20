FROM ubuntu:20.04                                    

ENV TERM=xterm

# send over selections
COPY files/deb-selections /deb-selections
RUN debconf-set-selections /deb-selections

# perform packages install
COPY files/packages /packages                                                       
RUN apt-get update && \
    xargs -t -a /packages apt-get install -y && \
    apt-get clean

# add internal CA certificate
COPY files/internal-ca.crt /usr/local/share/ca-certificates/internal-ca/internal-ca.crt                                                                              
RUN update-ca-certificates

# send over sudoers file for proxy user
COPY files/proxy /etc/sudoers.d/proxy

# send over config
COPY conf.d /etc/squid/conf.d

EXPOSE 3128/tcp

USER proxy
ENTRYPOINT ["/usr/bin/sudo", "/usr/sbin/squid", "--foreground"]
