FROM drupal:8.5.0

RUN apt-get update
RUN apt-get -y install supervisor
RUN apt-get -y install iputils-ping
RUN apt-get -y install libssl-dev

ADD supervisord.conf /etc/supervisor/conf.d/supervisord.conf 

COPY drupal-8.5.0 /var/www/html
RUN mkdir /opt/net-diag
COPY net-diag /opt/net-diag
COPY drupal-8.5.0 /var/www/html

RUN cd /var/www/html/ && g++ red_ch1.cpp -lcrypto -o red_ch1.flag && rm -f red_ch1.cpp
RUN cd /opt/net-diag/ && g++ red_ch2.cpp -lcrypto -o red_ch2.flag && rm -f red_ch2.cpp


RUN mkdir /root.bkp && mkdir /root.bkp/.aws
COPY redchallenge3lambda/red_ch3.flag /root.bkp/.aws/
RUN chown -R www-data:www-data /root.bkp

RUN mkdir /root/.aws
COPY redchallenge3lambda/blue_ch3.flag /root/.aws/

ARG accessKey
ARG secretAccessKey

RUN echo "[default] \n aws_access_key_id=$accessKey \n aws_secret_access_key=$secretAccessKey" > /root/.aws/credentials
RUN echo "[default] \n region = us-east-2" > /root/.aws/config
RUN echo "[default] \n aws_access_key_id=$accessKey \n aws_secret_access_key=$secretAccessKey" > /root.bkp/.aws/credentials
RUN echo "[default] \n region = us-east-2" > /root.bkp/.aws/config

WORKDIR /var/www/html

RUN set -eux; \
	chown -R www-data:www-data sites modules themes

ENTRYPOINT ["/usr/bin/supervisord"]
