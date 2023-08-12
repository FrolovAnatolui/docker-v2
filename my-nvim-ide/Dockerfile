# Установка базового образа
FROM ubuntu:latest
#FROM alpine:edge
#FROM ubuntu:20.04
#FROM ubuntu:18.04
# Обновление пакетов и установка зависимостей
RUN apt-get update && apt-get install -y \
    software-properties-common \
    curl \
    git \
    && rm -rf /var/lib/apt/lists/*

# Установка nvim
RUN add-apt-repository ppa:neovim-ppa/stable && \
    apt-get update && \
    apt-get install -y \
        neovim \
        libncurses5-dev \
        libncursesw5-dev \
        python3-dev \
        python3-pip
#COPY ~/.config/nvim/init.vim /root/.config/nvim/init.vim
RUN curl -fLo /root/.config/nvim/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim

RUN apt update && apt install -y build-essential
RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh | bash
RUN apt install -y tmuxinator
RUN apt-get update && apt-get install -y libffi-dev
RUN git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf && \
    ~/.fzf/install --all

#  Node.js
RUN export NVM_DIR="$HOME/.nvm" && \
    [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" && \
    [ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion" && \
    nvm install node
RUN apt-get install -y npm
#RUN nvim +CocInstall coc-python
# 
#RUN node -v && npm -v
# v20.5.0 
# 9.8.0

#RUN npm install
RUN npm install -g python
RUN curl -L https://github.com/rust-analyzer/rust-analyzer/releases/latest/download/rust-analyzer-linux -o rust-analyze
RUN chmod +x rust-analyze
RUN mv rust-analyze /usr/local/bin
RUN mkdir -p ~/.config/nvim/
RUN sh -c 'curl -fLo "${XDG_DATA_HOME:-$HOME/.local/share}"/nvim/site/autoload/plug.vim --create-dirs https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim'
#RUN nvm install node
# Установка плагинов для nvim (если необходимо)
#RUN nvim +PlugInstall +qall

# Копирование конфигурационных файлов (если необходимо)
# COPY .vimrc /root/.vimrc

# Запуск nvim при старте контейнера
#ENTRYPOINT tail -f /dev/null/
# zsh installation
RUN apt-get install -y zsh
CMD sleep infinity
EXPOSE 80 443
