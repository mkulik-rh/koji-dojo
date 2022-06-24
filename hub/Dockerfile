FROM quay.io/centos/centos:stream8
LABEL maintainer="mkulik@redhat.com"

#RUN sed -i '/excludedocs/d' /etc/rpm/macros.imgcreate
# RUN sed -i '/nodocs/d' /etc/yum.conf

# VOLUME ["/opt/koji-clients", "/opt/koji"]

RUN dnf -y update
RUN dnf -y install \
        sudo \
        nano \
        epel-release \
        nano \
        git \
        tar \
        bzip2 \
        rpm-build \
        make \
        patch \
        mod_ssl \
        python3-devel \
        python3-pip \
        python3-wheel \
        python3-setuptools

# Go to koji dir and build RPMs
# Clone the conf files into the docker container
WORKDIR "/opt/"
RUN git clone https://pagure.io/koji.git
WORKDIR "/opt/koji"
RUN make test-rpm
WORKDIR "/opt/koji/noarch"

# Install Koji RPM-s
RUN dnf -y install \
    koji-hub*.rpm \
    koji-1*.rpm \
    koji-web*.rpm \
    python3-koji-*.rpm

# Modify koji-web/hub config
COPY configs/web.conf /etc/kojiweb/web/
COPY configs/hub.conf /etc/koji-hub/

# Add koji pki
RUN mkdir -p /etc/pki/koji
COPY configs/pki-ssl.cnf /etc/pki/koji/ssl.cnf

# Modify sudoers
COPY configs/sudoers-apache /etc/sudoers.d/apache

# Copy httpd config files
COPY configs/httpd/conf.d/ /etc/httpd/conf.d/



# ADD etc/ /etc/
# ADD bin/ /usr/local/bin/
# ADD root/ /root/

# RUN chmod 600 /root/.pgpass
# RUN chmod +x /usr/local/bin/*

# ADD cgi/*.py /var/www/cgi-bin/
# RUN chmod o+rx /var/log /var/log/httpd
# RUN chmod +x /var/www/cgi-bin/*.py
# RUN chmod o+rwx /var/www/html
# RUN chmod -R o+rx /etc/httpd

# RUN mkdir -p /mnt/koji/{packages,repos,work,scratch}
# RUN chown apache.apache /mnt/koji/*
# RUN echo 'root:mypassword' | chpasswd

# EXPOSE 22 80 443

# ENTRYPOINT /usr/local/bin/entrypoint.sh
