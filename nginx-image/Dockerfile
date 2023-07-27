# Download base image ubuntu 20.04
FROM ubuntu:20.04
#FROM ubuntu:latest
#Let's add detailed information about the user image using the LABEL instruction.
LABEL maintainer="frolovanatolui@gmail.com"
LABEL version="0.1"

# Using ‘DEBIAN_FRONTEND = noninteractive’ prevents interactive queries when running the apt command
ARG DEBIAN_FRONTEND=noninteractive

# update ubuntu software repository
RUN apt-get update && \
apt dist-upgrade -y 

RUN apt install build-essential -y  curl file git
RUN apt-get install -y vim
RUN apt-get install -y xclip
RUN apt-get install -y tmux
RUN apt-get install -y tmuxinator
RUN export EDITOR='vim'
#RUN apt-get install -y 
#RUN apt install git-core curl fonts-powerline

# installing Nginx, PHP-FPM, supervisor packages 
# After the installation is complete, in order to reduce the size of the user image, we will delete the cache of all packages
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
COPY default ${nginx_vhost}
RUN sed -i -e 's/;cgi.fix_pathinfo=1/cgi.fix_pathinfo=0/g' ${php_conf} && \
echo "\ndaemon off;" >> ${nginx_conf} 

# Copy supervisor configuration
COPY supervisord.conf ${supervisor_conf}

# Define a volume for the custom image so that we can mount all these directories on the host machine.
# Volume configuration
VOLUME ["/etc/nginx/sites-enabled", "/etc/nginx/certs", "/etc/nginx/conf.d", "/var/log/nginx", "/var/www/html"]

# Now let's add the script start.sh and define the default container command using the CMD instruction as shown below.
# Copy start.sh script and define default command for the container
COPY start.sh /start.sh
CMD ["./start.sh"]

# Expose Port for the Application
EXPOSE 80 443
