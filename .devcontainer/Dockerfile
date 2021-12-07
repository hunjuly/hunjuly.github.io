FROM node:16

RUN apt update
RUN apt upgrade -y

# color prompt
RUN echo 'PS1="\[\033[32m\]\w\[\033[00m\] "' >> ~/.bashrc
RUN echo "alias ls='ls --color=auto'" >> ~/.bashrc
RUN echo "alias l='ls --color=auto -lA'" >> ~/.bashrc

RUN apt-get install -y ruby-full
RUN gem install bundler jekyll
