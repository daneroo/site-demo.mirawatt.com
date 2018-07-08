FROM nimmis/apache-php5:latest
RUN rm /var/www/html/index.html
COPY ./info.php /var/www/html/
COPY ./httpdocs /var/www/html/

## upgrade wordpress...
COPY ./wordpress-4.9.7.zip /tmp
RUN cd /tmp && unzip -x wordpress-4.9.7.zip
RUN cd /var/www/html/ && \
  rm -rf wp-admin/ && \
  rm -rf wp-includes/ && \
  cp -rp /tmp/wordpress/wp-admin . && \
  cp -rp /tmp/wordpress/wp-includes . && \
  cp -rp /tmp/wordpress/wp-content/ wp-content/ && \
  cp -p /tmp/wordpress/*php /tmp/wordpress/*html /tmp/wordpress/license.txt .

## Add static export plugins
COPY ./plugins/simply-static.2.1.0.zip /tmp/
COPY ./plugins/static-html-output-plugin.4.0.zip /tmp/
RUN cd /var/www/html/wp-content/plugins/ && \
  unzip  /tmp/simply-static.2.1.0.zip && rm /tmp/simply-static.2.1.0.zip && \
  unzip  /tmp/static-html-output-plugin.4.0.zip && rm /tmp/static-html-output-plugin.4.0.zip && \
  mkdir -p /var/www/vhosts/mirawatt.com/httpdocs/com/mirawatt/demo/wp-content/uploads && \
  chmod a+rwx /var/www/vhosts/mirawatt.com/httpdocs/com/mirawatt/demo/wp-content/uploads