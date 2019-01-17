FROM wordpress:latest AS build

RUN mkdir temp
WORKDIR /temp

RUN curl -O https://xdebug.org/files/xdebug-2.6.1.tgz
RUN tar -xvzf xdebug-2.6.1.tgz  
WORKDIR /temp/xdebug-2.6.1

RUN phpize
RUN ./configure --enable-xdebug 
RUN make

RUN echo 'zend_extension="/usr/local/lib/php/extensions/no-debug-non-zts-20170718/xdebug.so"' > /temp/php.ini
RUN echo "xdebug.remote_enable=1" >> /temp/php.ini
RUN echo "xdebug.remote_autostart=1" >> /temp/php.ini
RUN echo "xdebug.remote_connect_back=1" >> /temp/php.ini
RUN echo "xdebug.remote_port=9000" >> /temp/php.ini

FROM wordpress:latest as final
COPY --from=build /temp/xdebug-2.6.1/modules/xdebug.so /usr/local/lib/php/extensions/no-debug-non-zts-20170718
COPY --from=build /temp/php.ini /usr/local/etc/php/php.ini
