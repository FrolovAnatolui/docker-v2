# Установка базового образа
FROM ubuntu:latest

# Обновление пакетов и установка зависимостей
RUN apt-get update && apt-get install -y \
    software-properties-common \
    curl \
    git \
    && rm -rf /var/lib/apt/lists/*

# Установка nvim
RUN add-apt-repository ppa:neovim-ppa/stable && \
    apt-get update && \
    apt-get install -y neovim

# Установка плагинов для nvim (если необходимо)
# RUN nvim +PlugInstall +qall

# Копирование конфигурационных файлов (если необходимо)
# COPY .vimrc /root/.vimrc

# Запуск nvim при старте контейнера
#CMD [ "nvim" ]
