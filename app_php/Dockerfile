FROM php:8.2-apache

RUN apt-get update && apt-get upgrade -y && rm -rf /var/lib/apt/lists/*

RUN docker-php-ext-install opcache \
    && echo "disable_functions = exec,passthru,shell_exec,system,proc_open,popen,curl_exec,curl_multi_exec,parse_ini_file,show_source" >> /usr/local/etc/php/conf.d/docker-php-disable-functions.ini

COPY index.php /var/www/html/

RUN chown -R www-data:www-data /var/www/html && chmod -R 750 /var/www/html

USER www-data

EXPOSE 80