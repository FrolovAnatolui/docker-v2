# Download base image ubuntu 20.04
FROM ubuntu:20.04
# Добавим подробную информацию о пользовательском образе, используя инструкцию LABEL.
LABEL maintainer="frolovanatolui@gmail.com"
LABEL version="0.1"
LABEL description="Обычный докер файл для многоконтейнерного docker-compose PHP-FPM and Nginx services"

# Использование ‘DEBIAN_FRONTEND = noninteractive’ предотвращает интерактивные запросы при запуске команды apt
ARG DEBIAN_FRONTEND=noninteractive

# update ubuntu software repository
RUN apt update

# Установка пакетов Nginx, PHP-FPM, supervisor 
# После завершения установки, чтобы уменьшить размер подьзовательского образа удалим кэш все пакетов
# Install nginx, php-fpm and supervisord from ubuntu repository
RUN apt install -y nginx php-fpm supervisor && \
rm -rf /var/lib/apt/lists/* && \
apt clean

# Define the ENV variable
ENV nginx_vhost /etc/nginx/sites-available/default
ENV php_conf /etc/php/7.4/fpm/php.ini
ENV nginx_conf /etc/nginx/nginx.conf
ENV supervisor_conf /etc/supervisor/supervisord.conf

# Enable PHP-fpm on nginx virtualhost configuration
# Теперь скопируем конфигурацию Nginx по умолчанию в переменную ‘nginx_vhost’, заменим в файле конфигурации php.ini’cgi.fix_pathinfo = 1′ на ‘cgi.fix_pathinfo = 0’ и добавим опцию ‘daemon off’ в ‘nginx_conf’.
COPY default ${nginx_vhost}
RUN sed -i -e 's/;cgi.fix_pathinfo=1/cgi.fix_pathinfo=0/g' ${php_conf} && \
echo "\ndaemon off;" >> ${nginx_conf} 

# Copy supervisor configuration
COPY supervisord.conf ${supervisor_conf}

# Определите том для пользовательского образа, чтобы мы могли смонтировать все эти каталоги на хост-машине.
# Volume configuration
VOLUME ["/etc/nginx/sites-enabled", "/etc/nginx/certs", "/etc/nginx/conf.d", "/var/log/nginx", "/var/www/html"]

# Теперь добавим скрипт start.sh и определим команду контейнера по умолчанию, используя инструкцию CMD, как показано ниже.
# Copy start.sh script and define default command for the container
COPY start.sh /start.sh
CMD ["./start.sh"]

# Expose Port for the Application
EXPOSE 80 443
