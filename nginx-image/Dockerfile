# Download base image ubuntu 20.04
FROM ubuntu:20.04
#FROM ubuntu:latest
# Добавим подробную информацию о пользовательском образе, используя инструкцию LABEL.
LABEL maintainer="frolovanatolui@gmail.com"
LABEL version="0.1"
LABEL description="Обычный докер файл для многоконтейнерного docker-compose PHP-FPM and Nginx services"

# Использование ‘DEBIAN_FRONTEND = noninteractive’ предотвращает интерактивные запросы при запуске команды apt
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

# Uses "Spaceship" theme with some customization. Uses some bundled plugins and installs some more from github
#RUN sh -c "$(wget -O- https://github.com/deluan/zsh-in-docker/releases/download/v1.1.5/zsh-in-docker.sh)" -- \
#    -t https://github.com/denysdovhan/spaceship-prompt \
#    -a 'SPACESHIP_PROMPT_ADD_NEWLINE="false"' \
#    -a 'SPACESHIP_PROMPT_SEPARATE_LINE="false"' \
#    -p git \
#    -p ssh-agent \
#    -p https://github.com/zsh-users/zsh-autosuggestions \
#    -p https://github.com/zsh-users/zsh-completions


# Установка пакетов Nginx, PHP-FPM, supervisor 
# После завершения установки, чтобы уменьшить размер подьзовательского образа удалим кэш все пакетов
# Install nginx, php-fpm and supervisord from ubuntu repository
RUN apt install -y nginx php-fpm supervisor && \
rm -rf /var/lib/apt/lists/* && \
apt clean

#RUN apt-get update
#RUN apt-get install -y zsh
run curl -LO https://github.com/neovim/neovim/releases/latest/download/nvim.appimage
RUN chmod u+x nvim.appimage
CMD ["./nvim.appimage"]

RUN apt-get update && \
    apt-get install -y neovim
CMD ["nvim"]
# Install nvm
#RUN git clone http://github.com/creationix/nvm.git /root/.nvm;
#RUN chmod -R 777 /root/.nvm/;
#RUN bash /root/.nvm/install.sh;
#RUN export NVM_DIR="$HOME/.nvm";
#RUN echo "[[ -s $HOME/.nvm/nvm.sh ]] && . $HOME/.nvm/nvm.sh" >> $HOME/.bashrc;
#RUN bash -i -c 'nvm ls-remote';
#RUN apt update -y &&  apt upgrade -y && apt install wget bash -y
#RUN wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
#



#SHELL ["/bin/bash", "--login", "-c"]

#RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash
#RUN bash -i -c 'nvm install 10.15.3'





# Обновление пакетов и установка зависимостей
##RUN apt-get update && apt-get install -y \
#    software-properties-common \
#    curl \
#    git \
#    && rm -rf /var/lib/apt/lists/*
#
## Установка nvim
#RUN add-apt-repository ppa:neovim-ppa/stable && \
#    apt-get update && \
#    apt-get install -y neovim


#RUN /bin/bash -i -c "source ~/.nvm/nvm.sh && nvm ls-remote"
#RUN sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" || true
#RUN apt-get update && \ 
#    apt-get install -y software-properties-common && \
#    add-apt-repository ppa:deadsnakes/ppa && \
#    apt-get update
#
#RUN apt-get install -y zsh
# Default powerline10k theme, no plugins installed
#RUN sh -c "$(wget -O- https://github.com/deluan/zsh-in-docker/releases/download/v1.1.5/zsh-in-docker.sh)"

# Uses "Spaceship" theme with some customization. Uses some bundled plugins and installs some more from github
RUN sh -c "$(wget -O- https://github.com/deluan/zsh-in-docker/releases/download/v1.1.5/zsh-in-docker.sh)" -- \
    -t https://github.com/denysdovhan/spaceship-prompt \
    -a 'SPACESHIP_PROMPT_ADD_NEWLINE="false"' \
    -a 'SPACESHIP_PROMPT_SEPARATE_LINE="false"' \
    -p git \
    -p ssh-agent \
    -p https://github.com/zsh-users/zsh-autosuggestions \
    -p https://github.com/zsh-users/zsh-completions
CMD ["sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"]
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
